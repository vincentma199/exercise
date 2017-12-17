=============================
DHCP练习题3 - Neutron DHCP操作
=============================

1. 查看net1 MTU值
================

命令
----

    neutron net-show net1

注意
----

    记录net1的mtu值

2. 查看dnsmasq进程
================

命令
----

    ps -ef | grep dnsmasq | grep <net1 uuid>

注意
----

    查看dnsmasq后面跟的参数里面，有mtu相关的dhcp option，数值与第一步的mtu相符

3. 登录虚机vm1
=============

根据视频登录虚机console

4. 查看虚机网卡信息
=================

命令
----

    ifconfig

注意
----

    查看eth0的mtu值，与第1，2步相符

5. 查看subnet gateway ip
=======================

命令
----

    neutron subnet-list

根据网段地址找到11.0.0.0/24对应的subnet，记录uuid

    neutron subnet-show <subnet uuid>

查看gateway ip

6. 查看dnsmasq配置信息
====================

命令
----

    cd /opt/stack/data/neutron/dhcp/<net1 uuid>

    cat opts

注意
----

查看option:router的值与subnet的gateway ip对应

7. 查看虚机的默认路由
==================

回到步骤4的窗口

命令
----

    ip r

注意
----

默认路由中的网关地址与步骤5，6中的gateway ip对应

8. 回到步骤6窗口
==============

命令
----

    neutron subnet-update <subnet uuid> --dns-nameserver 8.8.8.8

subnet uuid从步骤5获取

9. 触发dhcp更新
==============

回到步骤7窗口

命令
----

    sudo cirros-dhcpc up eth0

    cat /etc/resolv.conf

注意
----

    查看虚机的dns server已经指向了8.8.8.8