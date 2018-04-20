====================
实验2-7 L2 population和ARP proxy
====================

1. 创建网络
==================

    source ~/devstack/openrc admin admin

    neutron net-create vxlan

    neutron subnet-create vxlan 10.0.0.0/24

2. 创建虚拟机
============

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=vxlan --availability-zone nova:openstack-controller vm1

    nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=vxlan --availability-zone nova:openstack-compute vm2
    
等待虚机状态变成ACTIVE
    
    nova console-log vm1
    
    nova console-log vm2
    
查看虚机启动日志，等待虚机启动完成，并提示登录。

3. 查看L2 population的OpenFlow流表
=============

在controller节点上：

    sudo ovs-ofctl dump-flows br-tun table=20

4. tcpdump controller网卡
==========

在controller节点上执行：

    sudo tcpdump -nei ens33 | grep -1 VXLAN
    
注意，实验环境网卡名可能不一样

5. 从vm1发送ARP广播
===========

登录Horizon，Horizon用户名密码是admin/nomoresecret
在虚机vm1内：

    sudo arping -c 1 <vm2 ip 地址>

注意观察第四步内容，可以看到controller节点上的网卡发出了arp广播包

6. 打开arp responder功能
==============

在controller和compute节点上都执行：

    vim /etc/neutron/plugins/ml2/ml2_conf.ini
    
在[agent] 配置分组下，添加

    arp_responder = True
    
重启neutron-openvswitch-agent

    sudo systemctl restart devstack@q-agt

7. 重新查看arp 广播包
============

重复步骤4，5，确定在controller节点的网卡上，看不到arp广播包。

8. 清理环境
==========

    nova delete vm1 vm2
    
    neutron delete vxlan
