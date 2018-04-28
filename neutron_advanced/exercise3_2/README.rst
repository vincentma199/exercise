====================
实验3-2 - DVR东西向流量
====================

1. 创建相应的网络
==================

    neutron net-create net1
    
    neutron net-create net2
    
    neutron subnet-create --name subnet1 net1 11.0.0.0/24
    
    neutron subnet-create --name subnet2 net2 12.0.0.0/24

2. 创建路由器，连接相应的网络
==============

    neutron router-create dvr
    
    neutron router-interface-add dvr subnet1
    
    neutron router-interface-add dvr subnet2
    
3. 创建虚拟机
==========

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-compute vm1
    
    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net2 --availability-zone nova:openstack-compute vm2
    
    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net2 --availability-zone nova:openstack-controller vm3

等待vm1，vm2启动完成

    nova console-log vm1
    
    nova console-log vm2
    
    nova console-log vm3
    
直到输出登录信息才说明虚机启动完成

4. 观察分布式网关
=======

在计算节点和控制节点，可以看到同一个路由器的namespace，并且里面的路由器网卡信息一样。

5. 从horizon登陆虚机
==========

    参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内：
    
    ping <vm2 ip address>

6. 观察数据包
=======

compute上的router namespace里面，可以看到三层转发数据，

但是不同于legacy router，在controller上看不到任何数据。说明同一个计算节点的三层转发，在本地就完成了。


7. 从vm1 ping vm3
=====

    返回步骤4，在虚机vm1内部：
    
    ping <vm3 ip address>
    
8. 观察数据包
========

compute上的router namespace里面，可以看到三层转发数据，但是只有vm1发出的部分。

controller的router namespace里面，可以看到vm3返回的数据。

这里是非对称路由。
