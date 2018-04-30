====================
实验3-5 - DVR SNAT 路径分析
====================
      
1. 创建Private network
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
 
5. 从horizon登陆虚机
==========

参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内：
    
    ping 8.8.8.8

6. 在compute节点的router namespace内观察网络数据
============
 
在路由器namespace内，
 
    tcpdump -nei qr-xxxx
    
确认路由器内部经过三层转发，发给了sg-xxx

7. 在controller的snat namespace内观察网络数据
==================

在snat namespace内，
 
    tcpdump -nei sg-xxxx
    
    tcpdump -nei qg-zzz

8. 在compute节点的虚机网卡上观察网络数据
==========

    sudo tcpdump -nei tap-xxxxx
    
从抓包的内容看，确认sg直接将网络数据发送给了虚机
