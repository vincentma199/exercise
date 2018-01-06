======================
租户联系题1 - Tenant基础
======================

1. 登录demo租户，demo用户
=======================

    source devstack/openrc demo demo

2. 查看网络列表
=============

    neutron net-list

确认看不到net1, net2

3. 创建网络
==========

    neutron net-create net3

4. 查看网络列表
=============

    neutron net-list

确认可以看到net3

5. 登录demo租户，admin用户
========================

    source devstack/openrc admin demo

6. 查看网络列表
=============

    neutron net-list

确认可以看到net1, net2, net3

7. 登录admin租户，admin用户
=========================

    source devstack/openrc admin demo

8. 查看网络列表
=============

    neutron net-list

确认可以看到net1, net2, net3

9. 删除net3
==========

    neutron net-delete net3

在admin的租户下，删除了demo租户下创建的net3.