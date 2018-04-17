====================
实验2-4 - 创建VLAN网络
====================

1. 查看controller节点上，查看虚机对应的local vlan
==================

    sudo ovs-vsctl show | grep -E "Bridge|Port|options|tag"
    
记录虚机端口（qvoxxxxx）对应的local vlan

2. 查看controller节点上，br-int的OpenFlow流表
==============

    sudo ovs-ofctl dump-flows br-int
    
根据课程内容分析流表内容

3. 查看controller节点上，br-int的mac地址表
==========

    sudo ovs-appctl fdb/show br-int

根据课程内容，分析mac地址表内容
    
4. 查看controller节点上，br-tun的OpenFlow流表
==========

    sudo ovs-ofctl dump-flows br-tun
    
根据课程内容分析流表内容
    
5. 查看compute节点上，br-tun的OpenFlow流表
=========

    sudo ovs-ofctl dump-flows br-tun
    
根据课程内容分析流表内容
    
6. 查看compute节点上，br-int的OpenFlow流表
========

    sudo ovs-ofctl dump-flows br-int
        
根据课程内容分析流表内容
    
7. 查看compute节点上，br-int的mac地址表
========

    sudo ovs-appctl fdb/show br-int
    
根据课程内容，分析mac地址表内容
    
8. 查看compute节点上，虚机对应的local vlan
===========

    sudo ovs-vsctl show | grep -E "Bridge|Port|options|tag"

记录虚机端口（qvoxxxxx）对应的local vlan
    
    
