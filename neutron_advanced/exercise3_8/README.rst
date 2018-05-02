====================
实验3-8 - Legacy Router Floatingip iptables
====================

在前一个实验的基础上

1. 观察router namespace里面的iptables
============================

进入router namespace

    sudo ip netns exec <路由器namespace> bash
    
    iptables -t nat -S
    
根据课程内容，分析并查看相应的iptables规则。

2. 清理环境
==============

    nova delete vm1

    neutron router-interface-delete legacy subnet1

    neutron router-delete legacy

    neutron net-delete net1
    
    neutron floatingip-list
    
查看所有floatingip，

    neutron floatingip-delete <floatingip uuid>
