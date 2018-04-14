====================
实验2-1 - 创建FLAT网络
====================

0. 删除之前实验的资源
==============

    nova list
    
如果有任何虚机，删除

    nova delete <vm>
    

    neutron net-list
    
如果有任何网络，删除

    neutron net-delete <net>


1. 查看虚拟交换机的连接
==================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options"

2. 创建flat网络
==============

    neutron net-create vlan100 --provider:network_type vlan --provider:physical_network public --provider:segmentation_id 100
    
    neutron subnet-create vlan100 192.168.31.0/24 --name vlan100-subnet

3. 添加虚机
====

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=vlan100 --availability-zone nova:openstack-controller vm1
    
等待虚机状态变成ACTIVE
    
    nova console-log vm1
    
查看虚机启动日志，等待虚机启动完成，并提示登录
    

4. 登录虚机
====

打开浏览器，输入controller IP地址，登录Horizon，用户名密码是admin/nomoresecret，根据视频内容找到vm1，并从console登录。从虚机内部ping 计算节点。
    
5. 监听controller网卡
=========

在controller节点上，执行

    sudo tcpdump -nei -vvv ens33

注意，实验环境的网卡名可能不一样，需要确认。
观察网络数据的vlan tag。
    
    
