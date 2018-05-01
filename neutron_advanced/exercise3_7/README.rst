====================
实验3-7 - Legacy Router Floatingip
====================
      
1. 创建Private network
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

5. 关联floatingip
==============

    neutron port-list --device-id <vm1 uuid>
    
获得相应的port的uuid

    neutron floatingip-create public --port-id <port uuid>

6. 从horizon登陆虚机
==========

参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内：
    
    ping 8.8.8.8


7. 观察router namespace里面的网络数据
================

进入router namespace

    sudo ip netns exec <路由器namespace> bash
    
    tcpdump -nei qr-xxx
    
    tcpdump -nei qg-yyy

8. 从外网直接访问floatingip
==============

在controller节点上，

    ping <floatingip 地址>
    
    ssh cirros@<floatingip 地址>
    
虚机密码是cubswin:)

在虚拟机内部，看不到floatingip对应的网卡，虚拟机内部网卡还是一个私网ip地址
