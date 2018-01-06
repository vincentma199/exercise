=========================
租户练习题2 - 安全组和Tenant
=========================

1. 登录demo租户，demo用户
=======================

    source devstack/openrc demo demo

2. 创建网络虚机
=============

    neutron net-create net3
    neutron subnet-create net3 33.0.0.0/24
    nova boot --image <cirros image uuid> --flavor 1 --nic net-name=net3 vm3

3. 登录admin租户，admin用户
=========================

    source devstack/openrc admin admin

4. 添加router interface
=======================

    neutron router-interface-add router2 <net3的subnet的uuid>

5. 删除icmp对应的安全组规则
========================

前面的实验过程手动的添加了这个规则，为了这个实验，需要删除这条规则

    neutron security-group-rule-list

找到对应的icmp规则的uuid

    neutron security-group-rule-delete <安全组规则uuid>

6. 创建虚机vm4
=============

    nova boot --image <cirros image uuid> --flavor 1 --nic net-name=net2 vm4

7. 登录vm1
=========

    ping vm4地址，可以ping通。ping vm3地址，不能ping通。

同样具有三层连接的几个虚机，但是因为安全组的一个默认行为，不同租户之间的虚机流量被隔离了，同一个租户下的虚机流量被放行。