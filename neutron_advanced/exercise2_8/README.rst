====================
实验2-8 - VLAN aware VM
====================

1. 创建相应的网络
==================

    openstack network create parent-net
    
    openstack subnet create parent-subnet --network parent-net --subnet-range 20.0.0.0/24
    
    openstack network create sub-net
    
    openstack subnet create sub-subnet --network sub-net --subnet-range 21.0.0.0/24

2. 创建trunk port和sub port
==============

    openstack port create parent-port1 --network parent-net
    
    openstack port create sub-port1 --network sub-net
    
    openstack network trunk create trunk1 --parent-port parent-port1
    
    openstack network trunk set trunk1 --subport port=sub-port1,segmentation-type=vlan,segmentation-id=102
    
    openstack port create parent-port2 --network parent-net
    
    openstack port create sub-port2 --network sub-net
    
    openstack network trunk create trunk2 --parent-port parent-port2
    
    openstack network trunk set trunk2 --subport port=sub-port2,segmentation-type=vlan,segmentation-id=102

3. 用两个parent port创建两个虚机
==========

    nova boot --image ubuntu --flavor d2 --nic port-id=<parent-port1 id> --key-name=openstack-controller --availability-zone nova:openstack-controller vm1
    
    nova boot --image ubuntu --flavor d2 --nic port-id=<parent-port2 id> --key-name=openstack-controller --availability-zone nova:openstack-compute vm2

等待vm1，vm2启动完成

    nova console-log vm1
    
    nova console-log vm2
    
直到输出登录信息才说明虚机启动完成
    
4. 登录虚机vm1，配置vlan子网卡
==========

    sudo ip netns exec <qdhcp-xxxxx> bash

登录parent-net对应的dhcp namespace

    ssh -i ~/.ssh/id_rsa ubuntu@<vm1 ip地址>
    
在虚机内部执行下面命令，如果下面命令返回较慢，且提示“unable to resolve host vm1”, 在/etc/hosts中，localhost后面加上vm1

    sudo modprobe 8021q

    sudo ip link add link ens3 name ens3.102 type vlan id 102
    
    sudo ip link set dev ens3.102 address <sub-port1 mac address>
    
    sudo ip link set dev ens3.102 up

通过dhcp获取vlan子网卡ip地址

    sudo dhclient -v ens3.102 
    
观察命令的返回，确认可以获取sub-port1的IP地址。
    
5. 登录虚机vm2，配置vlan子网卡
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
    
    neutron net-delete vxlan
    
