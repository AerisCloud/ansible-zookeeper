ZooKeeper role
===============

This role automates the maintenance of a ZooKeeper cluster.

In the context of the softwares we use, this could currently be
leveraged as an alternative to either multicast or avahi/mDNS by the
following applications:

* MAGE games
* Elasticsearch clusters

It is possible to start one than one cluster of nodes in a single datacenter,
but it is unlikely to be required for our typical use cases.

Configuration
--------------

### inventory file

```ini
[zookeeper:children]
zookeeper1

[zookeeper1]
node1.zookeeper1.dev  ansible_ssh_host=172.16.0.101 zoo_id=1
node2.zookeeper1.dev  ansible_ssh_host=172.16.0.102 zoo_id=2
node3.zookeeper1.dev  ansible_ssh_host=172.16.0.103 zoo_id=3

[zookeeper1:vars]
cluster_name = zookeeper1 # Must be set to the name of the subgroup
zookeeper_client_port=2181 # optional
zookeeper_leader_port=2888 # optional
zookeeper_election_port=3888 # optional
```

### Task configuration example

```yaml
- name: Run the tasks for 'zookeeper' servers
  hosts: zookeeper
  gather_facts: true
  sudo: true
  user: '{{ user }}'
  accelerate: '{{ accelerated }}'   # if set to true, ensure accelerate_port is firewalled
  accelerate_port: 9210             # Note: always protect this port from outsiders
  roles:
    - common
    - zookeeper
```

Execution
----------

For any bootstrap or addition of node to the cluster, you must run
the playbook on the full cluster.

```bash
ansible-playbook game.yml -vvv -i inventory/development --limit="zookeeper-1"
```

See also
--------

* [Hortonworks installation minimal requirements](http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.1/bk_installing_manually_book/content/rpm-chap1-2.html)
* [Article on how to set up a ZooKeeper cluster](http://myjeeva.com/zookeeper-cluster-setup.html)
* [Ansible example for setting up ZooKeeper](https://github.com/ansible/ansible-examples/blob/master/hadoop/roles/zookeeper_servers/tasks/main.yml)
