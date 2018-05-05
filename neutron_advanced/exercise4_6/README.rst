====================
实验4-6 Allowed address pair
====================
      
 
1. 创建网络
==========

    neutron net-create net1
    
    neutron subnet-create --name subnet1 net1 10.0.0.0/24

2. 创建虚拟机
==========

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=net1 --availability-zone nova:openstack-controller vm1

3. 从horizon登陆虚机
==========

参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内，为网卡添加一个副ip

    sudo ip address add 10.0.0.100/24 dev eth0
    
4. 登录dhcp namespace, ping 10.0.0.100
========

    sudo ip netns exec qdhcp-xxxxx
    
    ping 10.0.0.100
    
5. 添加allowed_address_pair
=================

    neutron port-list --device-id <vm uuid>
    
    neutron port-update <port uuid> --allowed_address_pairs type=dict list=true ip_address=10.0.0.100/24

6. 查看ping的返回
==========

    回到步骤4，确认现在ping可以成功

7. 清理环境
================

    nova delete vm1

    neutron net-delete net1
