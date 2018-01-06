=======================
路由器练习题2 - 路由器实现
=======================

1. 创建路由器
============

命令
----

    neutron router-create router2

2. 为路由器添加interface
======================

命令
----

    neutron net-list

找到net1对应的11.0.0.0/24对应的subnet id

    neutron router-interface-add router2 <subnet uuid>

3. 查看路由器namespace
=====================

命令
----

    ip netns

路由器对应的namespace就是qrouter-<router uuid>

4. 登录路由器namespace
=====================

命令
---

    sudo ip netns exec qrouter-<router uuid> bash

查看路由器的网卡，找到步骤2中添加的子网对应的网卡

    exit

退出namespace，返回到操作系统

5. 为路由器新增一块网卡
====================

命令
----
    neutron net-create net2
    neutron subnet-create net2 22.0.0.0/24
    neutron router-interface-add router2 <新创建subnet的uuid>

6. 登录路由器namespace
====================

与步骤4一样，查看路由器新增网卡。

    exit

退出namespace，返回到操作系统

7. 为路由器添加gateway
====================

命令
----

    neutron router-gateway-set router2 public

8. 登录路由器namespace
====================

与步骤6一样，查看路由器新增网卡。

    exit

退出namespace，返回到操作系统

9. 添加路由器路由
===============


命令
----
    neutron router-update router2 --route destination=30.0.0.0/24,nexthop=11.0.0.4

10. 登录路由器namespace
====================

与步骤8一样，查看路由器网卡没有变化，查看路由器namespace的路由。

    ip route

查看新增的路由

    exit

退出namespace，返回到操作系统

11. 删除路由器路由
================

命令
----

    neutron router-update router2 --no-routes

12. 创建VM2
==========

命令
----

    nova boot --image <cirros镜像uuid> --flavor 1 --nic net-name=net2 vm2

13. 从VM1 ping VM2
==================

根据视频登录到VM1，并ping VM2的地址

14. 抓取VM1发出来的包
===================

根据4.1内容，找到虚机的tap设备，通过下面命令抓取tap设备流量

    tcpdump -nei <tap设备名>

注意看虚机发送出来的网络报文中，目的MAC地址是路由器的网卡对应的MAC地址，请大家分析原因。