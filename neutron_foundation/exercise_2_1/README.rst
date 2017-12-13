======================
安全组练习题1 - 防火墙
======================

1. 登录DHCP namespace
======================

命令
----

    neutron net-list

查看相应的network uuid

    ip netns

查看环境中的namespace

    sudo ip netns exec <qdhcp-network-uuid> bash

对比network uuid和namespace，找到namespace并登录进去

注意
----

登录进namespace之后可以查看一下网卡信息，可以发现与操作系统的网卡信息不一样

2. ping 虚机
============

命令
----

     ping <虚机 IP>


3. ssh 虚机
===========

新开一个terminal，重复步骤1，进入namespace

命令
----

    ssh cirros@<虚机 IP>

注意
----

登录密码默认是cubswin:)

4. 删除icmp安全组规则
====================

新开一个terminal，并且注入openstack用户名密码

    source ~/devstack/openrc admin admin

命令
----

    neutron security-group-rule-list

找到icmp对应的安全组规则

    neutron security-group-rule-delete <安全组规则uuid>

注意
----

回到步骤2窗口，观察ping，这时ping已经不能成功了

5. 验证虚机向外ping
===================

回到步骤3的窗口，从虚机ping DHCP server地址，ping可以成功

注意
----

请大家思考一下为什么从内向外ping能成功？

