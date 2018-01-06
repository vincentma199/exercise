====================================
路由器练习题3 - 路由器,网络，子网区别联系
====================================

1. 清理环境
============

删除vm1，vm2，以及net1中非11.0.0.0/24对应的子网。

2. 创建keypair
==============

命令
----

    nova keypair-add key > key.pem
    chmod 600 key.pem

3. 部署虚机
==========

命令
----

    nova boot --image ubuntu16 --flavor d1 --nic net-name=net1 --key-name key vm1

启动会比较慢，这里最多可能要等10分钟

4. 登录vm1
==========

新开一个窗口，登录router namespace。登录router namespace方法参考4.2

5. 登录虚机
==========

    ssh -i ubuntu@<vm1 ip地址>

注意，每个环境里面的vm1 IP地址可能不一样，可以通过nova list查看

    ip route

查看并记录虚机的路由

6. 为net1添加一个subnet
=====================

回到步骤3窗口

命令
----

    neutron subnet-create net1 12.0.0.0/24

7. 重新触发虚机dhcp请求
=====================

回到步骤5窗口

命令
----

    sudo systemctl restart networking

8. 查看虚机路由
=============

    ip route

查看虚机的路由，与步骤5进行对比，查看路由的变化，并分析原因。

9. 创建port
==========

查找步骤6中创建的subnet对应的uuid

命令
----
    neutron port-create net1 --fixec-ip subnet_id=<subnet uuid>

10. net1中部署新的虚机
====================

    nova boot --image ubuntu16 --flavor d1 --nic port-id=<步骤9中的port uuid> --key-name key vm2

11. 登录DHCP namespace
======================

新开一个窗口，根据net1的uuid找到namespace

命令
----

    sudo ip netns exec qdhcp-<net uuid> bash

12. 登录vm2
==========

    ssh -i key.pem ubuntu@<vm2 IP地址>

13. 从VM2 ping VM1
==================

ping vm1的IP地址

查看ping的返回，ping能成功，请大家分析原因。

14. 为路由器添加新的网卡
=====================

回到步骤10窗口，将步骤6中创建的子网添加的路由器

命令
----

    neutron router-interface-add router2 <步骤6 subnet uuid>

15. 登录router namespace
========================

参考4.2 登录router namespace

    tcpdump -nei <11.0.0.0/24对应的网卡>

16. 从VM2 ping VM1
==================

回到步骤13窗口，ping vm1的IP地址

17. 检查路由器的流量
==================

回到步骤15，查看并没有抓到任何包，请大家分析原因。