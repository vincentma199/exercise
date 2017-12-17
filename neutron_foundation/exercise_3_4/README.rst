=============================
DHCP练习题4 - Neutron DHCP租约
=============================

1. 修改neutron租约配置
====================

    vim /etc/neutron/neutron.conf

注意
----

找到dhcp_lease_duration，如果前面有#，去掉#，将值改为30. esc加wq退出vim。

2. 重启neutron dhcp agent
=========================

命令
----

    systemctl list-units devstack@*

查看所有devstack相关的进程，找到dhcp的进程

    systemctl restart devstack@q-dhcp.service

3. 更新虚机的dhcp配置
===================

登录虚机console，参考前面的介绍

命令
----

    sudo cirros-dhcpc up eth0

4. 修改subnet dns server
=======================

回到步骤2的窗口

命令
----

    neutron subnet-list

根据网段地址找到11.0.0.0/24对应的subnet，记录uuid

    neutron subnet-update <subnet uuid> --dns-nameserver 9.9.9.9

等待一段时间，大概30秒

5. 查看虚机网卡信息
=================

回到步骤3的窗口，查看dns server

命令
----

    cat /etc/resolv.conf

注意
----

查看dns server自动变成了9.9.9.9