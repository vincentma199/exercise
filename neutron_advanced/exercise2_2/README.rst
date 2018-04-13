====================
实验2-2 OpenFlow和Local VLAN
====================

1. 查看虚拟交换机的local vlan信息
==================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options|tag"
    
flat网络对应的dhcp端口和虚机端口都有相应的local vlan

2. 查看虚拟交换机的OpenFlow流表
============

    sudo ovs-ofctl dump-flows br-int
    
    sudo ovs-ofctl dump-flows br-ex
    
查看交换机的流表，根据视频内容分析流表的走向

3. 从horizon登陆虚机
=============

Horizon用户名密码是admin/nomoresecret
在虚机内ping一个物理网络地址

    sudo ovs-ofctl dump-flows br-int
    
    sudo ovs-ofctl dump-flows br-ex
    
查看OpenFlow流表统计信息的变化

4. 清理环境
==============

    source ~/devstack/openrc admin admin
    nova delete vm1
    neutron delete flat
