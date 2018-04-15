====================
实验2-4 VLAN网络数据流分析
====================

1. 查看虚拟交换机的local vlan信息
==================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options|tag"
    
vlan网络对应的dhcp端口和虚机端口都有相应的local vlan

2. 查看虚拟交换机的OpenFlow流表和mac地址表
============

    sudo ovs-ofctl dump-flows br-int
    
    sudo ovs-ofctl dump-flows br-ex
    
    sudo ovs-appctl fdb/show br-int
    
    sudo ovs-appctl fdb/show br-ex
    
查看交换机的流表，根据视频内容分析流表的走向

3. 清理环境
==============

    source ~/devstack/openrc admin admin
    
    nova delete vm1
    
    neutron delete vlan100
