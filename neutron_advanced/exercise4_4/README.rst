====================
实验4-4 QoS with Bandwidth limiter
====================
      
 
1. 创建QoS
==========

    openstack network qos policy create bw-limiter
    
    openstack network qos rule create --type bandwidth-limit --max-kbps 3000 --max-burst-kbits 2400 --egress bw-limiter

2. 创建网络，路由器，关联QoS
==========
    
    openstack network create net1 --qos-policy bw-limiter
    
    openstack subnet create subnet1 --network net1 --subnet-range 10.0.0.0/24 --dns-nameserver 8.8.8.8
    
    openstack router create dvr
    
    openstack router add subnet dvr subnet1

    openstack router set --external-gateway public dvr
    
3. 创建虚拟机
=========
    
    nova boot --image ubuntu --flavor d2 --nic net-name=net1 --availability-zone nova:openstack-controller --key-name=openstack-controller server
    
    nova boot --image ubuntu --flavor d2 --nic net-name=net1 --availability-zone nova:openstack-compute --key-name=openstack-controller client
    
3. 分别从dhcp namespace登录虚拟机
=================

    sudo ip netns exec qdhcp-xxxxx bash
    
    ssh -i ~/.ssh/id_rsa ubuntu@<server ip>
    
新开一个窗口

    sudo ip netns exec qdhcp-xxxxx bash
    
    ssh -i ~/.ssh/id_rsa ubuntu@<client ip>
    
4. 在两个虚拟机内部分别安装iperf3
==================

虚拟机之后，首先修改 /etc/hosts

    sudo vim /etc/hosts
    
在server虚机里面，localhost后面加上server，在client虚机里面，localhost后面加上client，之后都执行

    sudo apt update

    sudo apt install iperf3 -y

5. 在server虚机里执行
==========

    iperf3 -s -p 12345

6. 在client虚机里面执行
====================

    iperf3 -c <server ip> -p 12345 -t 50
    
从结果可以看出，带宽被限制在了3000K/s.

7. 清理环境
========

    nova delete vm1

    neutron router-interface-delete legacy subnet1
    
    neutron router-delete legacy

    neutron net-delete net1
