======================
DHCP练习题1 - DHCP理论
======================

1. 通过horizon登录虚机 
======================

根据视频登录虚机

2. 找到net1 uuid
================

命令
----

    neutron net-list

找到并记录net1的uuid

3. 登录DHCP namespace
=====================

命令
----

    sudo ip netns exec qdhcp-<net1 uuid> bash

    ip a

查看dhcp server的网卡名，在namespace内以tap开头

4. 监控dhcp server网卡名
========================

命令
----

    tcpdump -nei <dhcp server 网卡名>

5. 重新触发dhcp信息更新
=====================

回到步骤1的窗口

命令
----

    sudo cirros-dhcpc up eth0

注意
----

从命令行可以看到dhcp的更新信息

6. 查看dhcp server网卡的网络包
============================

回到步骤4的窗口，如果没有出现数据，ctrl+C退出tcpdump

查看一下tcpdump的输出，对比dhcp的四个过程