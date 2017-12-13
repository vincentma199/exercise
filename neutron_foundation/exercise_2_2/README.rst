==========================
安全组练习题2 - 定义安全组
==========================

1. 登录DHCP namespace
=====================

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
===========

新开一个terminal，并且注入openstack用户名密码

    source ~/devstack/openrc admin admin

命令
----

     ping <虚机 IP>

此时ping不到虚机

3. 找到虚机的对应的安全组
========================

命令
----

    neutron port-list

根据虚机的IP识别到虚机的port

    neutron port-show <port uuid>

查看port的security_group属性，找到对应的security group

4. 新增icmp安全组规则
====================

命令
----

    neutron security-group-rule-create <security group id> --direction ingress --ethertype ipv4 --protocol icmp --remote-ip-prefix 0.0.0.0/0

5. 验证ping虚机
===============

回到步骤1的窗口，ping 虚机地址，ping可以成功


6.新增租户
=========

回到步骤2的窗口

命令
----

    openstack project create test

7.查看默认安全组
================

命令
----

    neutron security-group-list --tenent-id <刚刚创建的project id>

注意
----

    查看安全组里面自带的四条安全组规则
