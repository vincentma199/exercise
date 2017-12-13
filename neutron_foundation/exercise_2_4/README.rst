==========================
安全组练习题4 - 使用安全组
==========================

1. 删除tcp 22安全组规则
=======================

命令
----

    neutron security-group-rule-list

找到tcp 22对应的安全组规则

    neutron security-group-rule-delete <安全组规则uuid>

2. 验证ping，ssh都失败
======================

新开一个terminal，登录DHCP namespace，如果前面有已经开的terminal，也可以用之前的

    ping <虚机ip>

    ssh -v cirros@<虚机ip>

3. 更新网络属性
===============

回到步骤1的窗口

命令
----

    neutron network-update net1 --port-security-enabled false

4. 创建新的虚机
===============

命令
----

    nova boot --image <cirros image uuid> --flavor 1 --nic net-name=net1 vm2

注意
----

    image uuid的查找方法在exercise_1有介绍

5. 查看虚机的port信息
=====================

命令
----

    neutron port-list --device-id <vm2 uuid>

拷贝port的uuid

    neutron port-show <port uuid>

查看port的属性，可以看到port-security-enabled为false，并且security_group为空

6. 验证ping，ssh到vm2成功
=========================

回到步骤2的窗口

    ping <vm2 ip>

    ssh -v cirros@<vm2 ip>

7. 为虚机端口添加安全组
=======================

回到步骤5的窗口

命令
----

    neutron port-list

根据IP地址找到并拷贝VM1 port的uuid

    neutron port-show <port uuid>

拷贝相应的security group uuid

    neutron port-update <vm2 port uuid> --port-security-enabled true --security-group <security group uuid>

注意
----

    vm2 port uuid在步骤5已经找到了

8. 验证ping，ssh到vm2失败
=========================

回到步骤6的窗口。如果窗口没有响应，新开一个terminal，重新进入DHCP namespace

    ping <vm2 ip>

    ssh -v cirros@<vm2 ip>

9. 验证vm1和vm2之间能通讯
=========================

根据视频，从horizon登陆vm1的console。从vm1 ping，ssh vm2。
可以看到ping和ssh vm2能成功

注意
----

大家思考一下为什么可以成功？

