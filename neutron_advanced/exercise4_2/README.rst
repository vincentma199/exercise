====================
实验4-2 Metadata Service with L3
====================
      
 
1. 创建网络，路由器
==========

    neutron net-create net1
    
    neutron subnet-create --name subnet1 net1 10.0.0.0/24
    
    neutron router-create --distributed False legacy
    
    neutron router-interface-add legacy subnet1

2. 创建虚拟机
==========

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-controller vm1
    
3. 在网络节点查看metadata相关进程
=================

    ps -ef | grep metadata
    
4. 查看router namespace网卡信息
==================

登录router namespace

    sudo ip netns exec qrouter-xxxxx bash
    
    ip a
    
169.254.169.254作为副ip挂在DHCP Server的网卡上

5. 从horizon登陆虚机
==========

参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内，查看虚机的路由：
    
    ip route
    
验证metadata服务可用

    curl 169.254.169.254
    
如果有返回说明可以成功访问metadata service

6. 清理环境
========

    nova delete vm1

    neutron net-delete net1
