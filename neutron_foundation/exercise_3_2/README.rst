=============================
DHCP练习题2 - Neutron DHCP实现
=============================

1. 查看环境中的namespace个数
==========================

命令
----

    ip netns

注意
----

    记录环境中的namespace个数

2. 创建一个新的网络
================

命令
----

    neutron net-create net2

记录net2的uuid

3. 查看环境中的namespace个数
==========================

命令
----

    ip netns

环境中的namespace个数并没有变化

4. 创建子网
==========

命令
----

    neutron subnet-create net2 20.0.0.0/24

5. 查看环境中的namespace个数
==========================

命令
----

    ip netns

net2对应的namespace被创建出来

6. 查看dnsmasq进程
=================

命令
----

    ps -ef | grep dnsmasq | grep <net2 uuid>

查看dnsmasq的各个参数

7. 进入dnsmasq参数目录
====================

命令
----

    cd /opt/stack/data/neutron/dhcp/<net2-uuid>

    cat host

注意
----

    记录host文件的内容

8. 新增虚机vm2
=============

命令
----

    nova boot --image <cirros image uuid> --flavor 1 --nic net-name=net2 vm2

等虚机状态变成ACTIVE

9. 进入dnsmasq参数目录
====================

命令
----

    cd /opt/stack/data/neutron/dhcp/<net2-uuid>

    cat host

注意
----

    查看host文件内容，新增了vm2的相关记录，第一项是mac地址，第二项是内部域名，第三项是IP地址

10. 恢复环境
==========

命令
----

    nova delete vm2

    neutron delete net2