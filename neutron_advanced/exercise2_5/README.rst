====================
实验2-5 - 创建VXLAN网络
====================

1. 查看虚拟交换机的连接
==================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options"

2. 创建vxlan网络
==============

    neutron net-create vxlan
    
    neutron subnet-create vxlan 10.0.0.0/24

3. 添加虚机
====

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=vxlan --availability-zone nova:openstack-controller vm1
    
    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=vxlan --availability-zone nova:openstack-compute vm2
    
等待虚机状态变成ACTIVE
    
    nova console-log vm1
    
    nova console-log vm2
    
查看虚机启动日志，等待虚机启动完成，并提示登录。

    nova list
    
记录两个虚机的IP地址。
    

4. 登录虚机
====

打开浏览器，输入controller IP地址，登录Horizon，用户名密码是admin/nomoresecret，根据视频内容找到vm1，并从console登录。从虚机内部ping vm2的IP地址。
    
5. 监听vm1网卡
=====

    neutron port-list
    
找到vm1对应的port的UUID。

在controller节点上

    ip a | grep <port UUID的前8个字节>

找到tap开头的设备，

    sudo tcpdump -nei tapxxxxxx
    
查看虚机发出的网络数据。

6. 监听controller网卡
=========

在controller节点上，执行

    sudo tcpdump -nei ens33 | grep VXLAN

注意，实验环境的网卡名可能不一样，需要确认。
观察网络数据的vxlan vni，这个与第二步创建的网络参数保持一致。
    
    
