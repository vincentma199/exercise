====================
实验3-9 - DVR Floatingip
====================
      
 
1. 修改计算节点的neutron-l3-agent配置
=================

登录到计算节点

    ssh <计算节点ip>
    
    vim /etc/neutron/l3_agent.ini
    
修改agent_mode为 dvr

    sudo systemctl restart devstack@q-l3
    
重启neutron-l3-agent使配置文件生效
 
2. 创建Private network
============

    neutron net-create net1
    
    neutron subnet-create --name subnet1 net1 10.0.0.0/24
    
3. 创建路由器，连接路由器和网路
=================

    neutron router-create dvr
    
    neutron router-interface-add dvr subnet1
    
    neutron router-gateway-set dvr public
    
4. 创建虚拟机
==========

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-compute vm1

5. 关联floatingip
==============

    neutron port-list --device-id <vm1 uuid>
    
获得相应的port的uuid

    neutron floatingip-create public --port-id <port uuid>

6. 从horizon登陆虚机
==========

参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内：
    
    ping 8.8.8.8
    
7. 在compute节点的router namespace内观察网络数据
============
 
在路由器namespace内，
 
    tcpdump -nei qr-xxxx
    
确认路由器内部经过三层转发，发给了sg-xxx

8. 在controller的snat namespace内观察网络数据
==================

在snat namespace内，
 
    tcpdump -nei sg-xxxx
    
    tcpdump -nei qg-zzz
    
    iptables -t nat -S


9. 清理环境
========

    nova delete vm1

    neutron router-interface-delete dvr subnet1

    neutron router-delete dvr

    neutron net-delete net1
    
    neutron floatingip-list
    
查看所有floatingip，

    neutron floatingip-delete <floatingip uuid>
