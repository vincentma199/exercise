====================
实验4-4 QoS with DSCP
====================
      
 
1. 创建QoS
==========

    openstack network qos policy create dscp-marking
    
    openstack network qos rule create --type dscp-marking --dscp-mark 26 dscp-marking

2. 创建网络，关联QoS
==========
    
    openstack network create net1 --qos-policy dscp-marking
    
    openstack subnet create subnet1 --network net1 --subnet-range 10.0.0.0/24
    
3. 创建虚拟机
=========
    
    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-controller vm1
    
等待虚机启动完成

    nova console-log vm1
    
4. 从horizon登陆虚机
==========

参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内，

    ping <dhcp server ip>

5. 在dhcp namespace内部，监听dhcp server
==========

    sudo ip netns exec qdhcp-xxxx bash
    
    tcpdump -v -nei tap-yyyyy

确认tos与前面设置的dscp一致。


6. 清理环境
========

    nova delete vm1
    
    neutron net-delete net1
    
    openstack network qos delete dscp-marking
