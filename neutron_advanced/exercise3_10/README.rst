====================
实验3-10 - DVR Floatingip
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
    
7. 在compute节点上观察虚拟交换机的端口
==========

    sudo ovs-vsctl show | grep -E "Bridge|Port|options|tag"
    
查看fg挂载的虚拟交换机

8. 在compute节点的router namespace内观察网络数据
============
 
在路由器namespace内，
 
    tcpdump -nei qr-xxxx
    
查看策略路由。

    ip rule
    
    ip route show table <table name>
    
查看iptables规则

    iptables -t nat -S
    
抓rfp网卡上的包

    tcpdump -nei rfp-zzz
    
退出路由器namespace

    exit

9. 在compute节点的fip namespace内观察网络数据
==================

在fip namespace内，
 
    tcpdump -nei fpr-zzz
    
查看策略路由。

    ip rule
    
    ip route show table <table name>
    
抓fg设备上的网络数据包

    tcpdump -nei fg-zzz
    
10. 验证proxy ARP
=======

在controller节点上执行

    arping -I br-ex <floating ip>
    
确认可以获取mac地址，且mac地址是fg设备的mac地址。

在计算节点的fip namespace内，查看proxy_arp的配置。

    sysctl -a | grep fg-xxx | grep proxy_arp

根据视频的演示，删除floatingip相关的路由。再回到controller节点上执行

    arping -I br-ex <floating ip>
    
确认无法获取mac地址。

11. 清理环境
========

    nova delete vm1

    neutron router-interface-delete dvr subnet1

    neutron router-delete dvr

    neutron net-delete net1
    
    neutron floatingip-list
    
查看所有floatingip，

    neutron floatingip-delete <floatingip uuid>
