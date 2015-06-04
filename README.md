ZooKeeper role
===============

This role automates the maintenance of a ZooKeeper cluster.

In the context of the software we use, this could currently be
leveraged as an alternative to either multicast or avahi/mDNS.

Configuration
--------------

### inventory file

```ini
[zookeeper:children]
zookeeper-somedc-prod

[zookeeper-somedc-prod:children]
zookeeper1-somedc-prod

[zookeeper1-somedc-prod]
node1.zookeeper.somedc.prod1  ansible_ssh_host=172.16.0.101 zoo_id=1
node2.zookeeper.somedc.prod1  ansible_ssh_host=172.16.0.102 zoo_id=2
node3.zookeeper.somedc.prod1  ansible_ssh_host=172.16.0.103 zoo_id=3

[zookeeper1-somedc-prod:vars]
# Must be set to the name of the subgroup
zookeeper_cluster_name = zookeeper1-somedc-prod
```

See also
--------

* [Hortonworks installation minimal requirements](http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.1/bk_installing_manually_book/content/rpm-chap1-2.html)
* [Article on how to set up a ZooKeeper cluster](http://myjeeva.com/zookeeper-cluster-setup.html)
