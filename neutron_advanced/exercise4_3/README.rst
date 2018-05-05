====================
实验4-3 配置Neutron QoS
====================
      
1. 配置控制节点
==========

    vim /etc/neutron/neutron.conf

在service_plugins后面加上qos

    vim /etc/neutron/plugins/ml2/ml2_conf.ini

在extension_drivers后面加上qos

在[agent]配置分组后面添加

    extensions = qos
    
之后重启所有neutron的进程

    sudo systemctl restart devstack@q-*

2. 配置计算节点
==========

    vim /etc/neutron/plugins/ml2/ml2_conf.ini
    
在[agent]配置分组后面添加

    extensions = qos
    
之后重启neutron-openvswitch-agent

    sudo systemctl restart devstack@q-agt

3. 创建qos policy
============

    neutron qos-policy-create test
    
4. 关联qos policy与network
===============

    neutron net-create net1 --qos-policy test

5. 清理环境
========

    neutron net-delete net1
    
    neutron qos-policy-delete test
