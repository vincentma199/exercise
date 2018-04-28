====================
实验3-3 - DVR host MAC
====================

1. 查看vm1对应的OpenFlow流表
==================
 
在compute节点，执行

    sudo ovs-ofctl dump-flows br-int
    
    sudo ovs-appctl fdb/show br-int
    
    sudo ovs-ofctl dump-flows br-tun
    
注意DVR host MAC观察
      
2. 查看vm3对应的OpenFlow流表
==============

在controller节点上，执行

    sudo ovs-ofctl dump-flows br-tun
    
    sudo ovs-ofctl dump-flows br-int

3. 从数据库查看DVR host MAC
==========

在controller节点，

    mysql
    
    use neutron;
    
    select * from dvr_host_macs;

4. 从horizon登陆虚机
==========

    参考视频登陆Horizon，用户名密码 admin/nomoresecret。在虚机vm1内：
    
    ping <vm3 ip address>

5. 观察数据包
=======

在controller节点上，抓网卡上的包

    sudo tcpdump -nei ens33 | grep -1 VXLAN
    
注意，大家的实验环境网卡名可能不一样。


6. 清理环境
=========

   nova delete vm1 vm2 vm3

    neutron router-interface-delete dvr subnet1

    neutron router-interface-delete dvr subnet2

    neutron router-delete dvr

    neutron net-delete net1

    neutron net-delete net2
