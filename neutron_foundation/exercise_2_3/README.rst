==========================
安全组练习题3 - 安全组实现
==========================

1. 查看网卡信息 
==============

命令
----

    neutron port-list

查看相应的port uuid

    ip a 

查看环境中与port uuid相关的网络设备

注意
----

找到qbr，qvb，qvo设备

2. 查看iptables
===============

命令
----

    sudo iptables -vnL

找到与port uuid对应的iptables规则，并且与安全组规则对应

3. 删除icmp安全组规则
=====================

命令
----

    neutron security-group-rule-list

找到icmp对应的安全组规则

    neutron security-group-rule-delete <安全组规则uuid>

注意
----

    对比iptables规则的变化
