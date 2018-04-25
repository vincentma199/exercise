====================
实验3-1 - Legacy router
====================

1. 创建相应的网络
==================

    neutron net-create net1
    
    neutron net-create net2
    
    neutron subnet-create --name subnet1 net1 11.0.0.0/24
    
    neutron subnet-create --name subnet2 net2 12.0.0.0/24

2. 创建路由器，连接相应的网络
==============

    neutron router-create --distributed False legacy
    
    neutron router-interface-add legacy subnet1
    
    neutron router-interface-add legacy subnet2

3. 创建虚拟机
==========

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-controller vm1
    
    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net2 --availability-zone nova:openstack-controller vm2

等待vm1，vm2启动完成

    nova console-log vm1
    
    nova console-log vm2
    
直到输出登录信息才说明虚机启动完成
    
4. 从horizon登陆虚机
==========

    参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机内：
    
    ping <vm2 ip address>

5. 记录neutron端口址
==========

参考步骤4完成。

6. 从vm1 ping vm2的vlan子网卡
=======

在vm1内部

    ping <sub-port2的ip地址>
    
在vm1，tcpdump ens3和ens3.102

    sudo tcpdump -nei ens3 icmp
    
    sudo tcpdump -nei ens3.102 icmp
    
注意观察vlan tag的信息

7. 清理环境
=====

    nova delete vm1 vm2
    
    openstack network trunk delete trunk1 trunk2
    
    openstack port delete parent-port1 parent-port2 sub-port1 sub-port2
    
    openstack network delete parent-net sub-net
    
