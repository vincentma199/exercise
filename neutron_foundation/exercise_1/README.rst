==================
neutron 基础练习题
==================

1. 创建网络
===========

命令
----

    neutron net-create net1

注意
----

查看创建网络的network_type和segmentation_id两个属性

2. 创建子网
===========

命令
----

    neutron subnet-create net1 <网络地址>

注意
----

网络地址可以任意填写，例如11.0.0.0/24

3. 创建虚机

命令
----

    glance image-list

先查看可用的镜像, 选用cirros镜像

    nova boot --image <image-id> --flavor 1 --nic net-name=net1 vm1

image-id用上一步得到的cirros镜像的uuid

    nova list

查看虚机状态，等到虚机状态变成ACTIVE，并且虚机分配了IP再进行下一步

4. 查看端口

命令
----

    neutron port-list --device-id <虚机id>

虚机id可以在上一步获取

注意
----

查看port的IP地址与虚机IP地址一致，记住port的mac地址

5. 登录虚机
===========

根据视频步骤登录虚机，检查虚机mac地址与上一步的port mac地址一致.

6. 检查二层网络
===============

根据视频步骤，从虚机ping DHCP server端口.
