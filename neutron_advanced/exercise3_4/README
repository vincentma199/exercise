====================
实验3-3 - DVR host MAC
====================

1. 查看vm1对应的OpenFlow流表
==================
 
在compute节点，执行

    sudo ovs-ofctl dump-flows br-int
    
    sudo ovs-appctl fdb/show br-int
    
    sudo ovs-ofctl dump-flows br-tun
    
注意DVR host MAC观察
      
2. 查看vm3对应的OpenFlow流表
==============

在controller节点上，执行

    sudo ovs-ofctl dump-flows br-tun
    
    sudo ovs-ofctl dump-flows br-int

3. 从数据库查看DVR host MAC
==========

在controller节点，

    mysql
    
    use neutron;
    
    select * from dvr_host_macs;

4. 从horizon登陆虚机
==========

参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内：
    
    ping <vm3 ip address>

5. 观察数据包
=======

在controller节点上，抓网卡上的包

    sudo tcpdump -nei ens33 | grep -1 VXLAN
    
注意，大家的实验环境网卡名可能不一样。


6. 清理环境
=========

   nova delete vm1 vm2 vm3

   neutron router-interface-delete dvr subnet1

   neutron router-interface-delete dvr subnet2

   neutron router-delete dvr

   neutron net-delete net1

   neutron net-delete net2
    neutron net-create public --provider:network_type flat --provider:physical_network public --router:external True

    neutron subnet-create public 192.168.31.0/24 --name public-subnet --allocation-pool start=192.168.31.200,end=192.168.31.254 --disable-dhcp

2. 创建Private network
============

    neutron net-create net1
    
    neutron subnet-create --name subnet1 net1 10.0.0.0/24
    
3. 创建路由器，连接路由器和网路
=================

    neutron router-create --distributed False legacy
    
    neutron router-interface-add legacy subnet1
    
    neutron router-gateway-set legacy public
    
4. 创建虚拟机
==========

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-controller vm1
    
5. 查看OpenVSwitch网桥信息
======================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options|tag"
    
6. 查看iptables规则
================

进入namespace

    sudo ip netns exec <路由器namespace> bash
    
    iptable -t nat -vnL
    
 注意看nat相关的iptables规则
 
 7. 从horizon登陆虚机
==========

    参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内：
    
    ping 8.8.8.8
    
 8. 观察沿途的网络数据包
 ============
 
 在路由器namespace内，
 
     tcpdump -nei qr-xxxx
     
     tcpdump -nei qg-yyy

9. 清理环境
==========

    nova delete vm1
    
    neutron router-interface-delete legacy subnet1
    
    neutron router-delete legacy
    
    neutron net-delete net1
