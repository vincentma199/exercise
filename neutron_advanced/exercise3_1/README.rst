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

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-compute vm1
    
    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net2 --availability-zone nova:openstack-compute vm2

等待vm1，vm2启动完成

    nova console-log vm1
    
    nova console-log vm2
    
直到输出登录信息才说明虚机启动完成

4. 查看OpenVSwitch网桥信息
======================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options|tag"

可以看到，在br-int上，路由器网卡qr-xxx与虚机的网卡没有区别。也带有相同的local VLAN。

5. 记录neutron端口址
==========

    neutron port-list
    
记录各个port对应的mac地址。


6. 从horizon登陆虚机
==========

    参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机内：
    
    ping <vm2 ip address>

7. 分别从虚机的网卡，路由器网卡观察网络数据包
=======

根据视频演示内容使用tcpdump进行观察。


8. 清理环境
=====

    nova delete vm1 vm2
    
    neutron router-interface-delete legacy subnet1
    
    neutron router-interface-delete legacy subnet2
    
    neutron router-delete legacy
    
    neutron net-delete net1
    
    neutron net-delete net2
