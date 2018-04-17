====================
实验2-1 - 创建FLAT网络
====================

1. 查看虚拟交换机的连接
==================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options"

2. 查看虚拟交换机连接的端口设备
============

    sudo ovs-ofctl dump-ports-desc br-int
    
    sudo ovs-ofctl dump-ports-desc br-ex

3. 查看虚拟交换机的mac地址表
=============

    sudo ovs-appctl fdb/show br-int
    
    sudo ovs-appctl fdb/show br-ex

4. 创建flat网络
==============

    neutron net-create flat --provider:network_type flat --provider:physical_network public
    
    neutron subnet-create flat 192.168.31.0/24 --name flat-subnet --allocation-pool start=192.168.31.200,end=192.168.31.254

5. 添加虚机
====

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=flat --availability-zone nova:openstack-controller vm1
    
等待虚机状态变成ACTIVE
    
    nova console-log vm1
    
查看虚机启动日志，等待虚机启动完成，并提示登录
    
6. 查看虚机信息
====

    nova show vm1
    
查看虚机当前的ip地址，和虚机被部署在哪个主机上“OS-EXT-SRV-ATTR:hypervisor_hostname”

7. 查看虚拟交换机的端口设备，mac地址表变化
===========

    sudo ovs-vsctl show | grep -E "Bridge|Port|options"
    
    sudo ovs-ofctl dump-ports-desc br-int
    
    sudo ovs-appctl fdb/show br-int


8. 直接登录虚机
====

    ssh cirros@<虚机ip地址>
    
9. 从虚机内访问控制节点和计算节点
===============

在虚机内部ping 控制节点ip和计算节点ip
 
    ping <控制节点ip>
    
    ping <计算节点ip>
    
    exit
    
退出虚拟机登录
