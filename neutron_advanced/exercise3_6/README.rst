====================
实验3-6 - DVR SNAT 策略路由
====================
      
1. 创建Private network
============

    neutron net-create net2
    
    neutron subnet-create --name subnet2 net2 11.0.0.0/24
    
3. 创建路由器，连接路由器和网路
=================
    
    neutron router-interface-add dvr subnet2

4. 创建虚拟机
==========

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net2 --availability-zone nova:openstack-compute vm2

5. 在compute节点的router namespace分析策略路由
============
 
在路由器namespace内，
 
    ip rule
    
查看相应的策略

    ip route show table <table name>
    
查看相应的路由表内容

6. 清理环境
==================

    nova delete vm1 vm2

    neutron router-interface-delete dvr subnet1

    neutron router-interface-delete dvr subnet2

    neutron router-delete dvr

    neutron net-delete net1
    
    neutron net-delete net2
