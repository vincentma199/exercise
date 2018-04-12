========================
二层网络实验1 - FLAT网络创建
========================

这是第一个操作类实验，在实验前，我们需要配置一下环境。

1. 打开DHCP的metadata中继功能
======================

    vim /etc/neutron/dhcp_agent.ini

在[DEFAULT]分组下添加一行配置：

enable_isolated_metadata = true

重启DHCP agent

    sudo systemctl restart devstack@q-dhcp.service

2. 打开相应的安全组规则
====================

命令
---
    source ~/devstack/openrc admin admin

    openstack project list
记录admin对应的uuid
    
    neutron security-group-list
记录admin租户的uuid对应的安全组uuid

    neutron  security-group-rule-create <安全组uuid> --protocol tcp --remote-ip-prefix 0.0.0.0/0
    
    neutron  security-group-rule-create <安全组uuid> --protocol icmp --remote-ip-prefix 0.0.0.0/0
打开ping和ssh通道
    
    
下面是实验正式内容：

1. router gateway
======

    neutron router-gateway-clear router1
    
2. 删除public网络
=====

    neutron net-delete public

3. 添加flat网络
====

    neutron net-create flat --provider:network_type flat --provider:physical_network public
    
    neutron subnet-create flat 192.168.31.0/24 --name flat-subnet --allocation-pool start=192.168.31.200,end=192.168.31.254

4. 添加虚机
====

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=flat vm1
等待虚机状态变成ACTIVE
    
    nova console-log vm1
查看虚机启动日志，等待虚机启动完成，并提示登录
    
5. 查看虚机信息
====

    nova show vm1
查看虚机当前的ip地址，和虚机被部署在哪个主机上“OS-EXT-SRV-ATTR:hypervisor_hostname”
    
6. 直接登录虚机
====

    ssh cirros@<虚机ip地址>
    
7. 从虚机内访问控制节点和计算节点
====

在虚机内部ping 控制节点ip和计算节点ip
    ping <控制节点ip>
    
    ping <计算节点ip>
    
8. 还原环境
====

    nova delete vm1
    
    neutron net-delete flat
    
    neutron net-create public --provider:network_type flat --provider:physical_network public --router:external True
    
    neutron subnet-create public 192.168.31.0/24 --name public-name --allocation-pool start=192.168.31.200,end=192.168.31.254 --disable-dhcp
    neutron router-gateway-set router1 public
