====================
实验3-4 - Legacy router SNAT
====================

1. 创建External provider network
==================

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

9. 观察Connection track内容
==================

在路由器namespace内，

    conntrack -L | grep icmp


10. 清理环境
==========

    nova delete vm1
    
    neutron router-interface-delete legacy subnet1
    
    neutron router-delete legacy
    
    neutron net-delete net1
