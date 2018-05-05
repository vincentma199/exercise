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
    
    openstack subnet create subnet1 --network net1 --subnet-range 10.0.0.0/24
    
3. 创建虚拟机
=========
    
    nova boot --image ubuntu --flavor d2 --nic net-name=net1 --availability-zone nova:openstack-controller --key-name=openstack-controller server
    
    nova boot --image ubuntu --flavor d2 --nic net-name=net1 --availability-zone nova:openstack-compute --key-name=openstack-controller client
    
等待虚机启动完成

    nova console-log server
    
    nova console-log client

3. 分别从dhcp namespace登录虚拟机
=================

    sudo ip netns exec qdhcp-xxxxx bash
    
    ssh -i ~/.ssh/id_rsa ubuntu@<server ip>
    
新开一个窗口

    sudo ip netns exec qdhcp-xxxxx bash
    
    ssh -i ~/.ssh/id_rsa ubuntu@<client ip>
    
4. （可选）配置/etc/hosts
==================

可选：如果登录虚机之后执行sudo命令反应过慢，可以修改 /etc/hosts

    sudo vim /etc/hosts
    
在server虚机里面，localhost后面加上server，在client虚机里面，localhost后面加上client


5. 在server虚机里执行
==========

    nc -v -l -p 12345

6. 在client虚机里面执行
====================

    dd if=/dev/zero bs=1024K count=20 | nc -v <server ip> 12345
    
从结果可以看出，带宽被限制在了375KB/s, 约等于3000Kb/s.

注，这里也可以用iperf3测试带宽，不过iperf3需要安装，在实验环境安装需要的额外配置较多，所以改用netcat测试带宽。

7. 清理环境
========

    nova delete server client

    neutron net-delete net1
