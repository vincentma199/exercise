========================
环境预配
========================

1. 安装controller
================

在controll虚机上，执行

    ip a
    
查看并记录当前虚机的IP地址，应该是192.168.31.X，查找当前的网卡，应该是ens3

    cd ~/devstack
    
    vim local.conf
    
找到PUBLIC_INTERFACE，将相应的值改成网卡名
找到HOST_IP，将相应的值改成虚机的IP地址

    ./stack.sh
    
等待脚本执行完成

2. 安装compute
=============

在compute虚机上，执行

    ip a
    
查看并记录当前虚机的IP地址，应该是192.168.31.X，查找当前的网卡，应该是ens3

如果发现网卡状态是DOWN，且没有IP地址，执行下面的命令启动网卡。

    sudo ifconfig ens3 up
    
    sudo dhclient ens3
    
之后部署计算节点

    cd ~/devstack
    
    vim local.conf
    
找到PUBLIC_INTERFACE，将相应的值改成网卡名
找到HOST_IP，将相应的值改成虚机的IP地址
找到SERVICE_HOST，将相应的值改成controller的IP地址。

    ./stack.sh
    
等待脚本执行完成

3. 添加compute节点
========

在controller虚机上执行

  ~/devstack/tools/discover_hosts.sh
  
  source ~/devstack/openrc admin admin
  
  nova hypervisor-list
  
确认可以看到两条记录

4. 添加ubuntu镜像
===============

在controller虚机上执行

    glance image-create --name ubuntu --file ~/devstack/files/xenial-server-cloudimg-amd64-disk1.img --disk-format qcow2 --container-format ovf --progress
