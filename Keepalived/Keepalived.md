# Keepalived

标签（空格分隔）： Keepalived

---

## 1. keepalived.conf

```
! Configuration File for keepalived

global_defs { # 全局定义
   notification_email {
     francs3@163.com # 定义通知邮件，可以设置多个，每行一个。需要开启sendmail服务
   }
   notification_email_from mail@163.com # 设置邮件的发送地址
   smtp_server 127.0.0.1 # 邮件服务器
   smtp_connect_timeout 30 # 设置SMTP Server的超时时间
   router_id base1 # 标识每个节点，每个节点不同
}

 vrrp_script check_pg_alived {
    script "/usr/local/bin/pg_moniter.sh"  # 健康检查脚本
    interval 10 # 健康检查周期
    fall 5 # 健康检查失败次数
    weight -1
    # 如果 < 0，执行健康检查正常返回(exit 0)，则 priority 不变，异常返回(exit 1)，则 priority = priority + weigtht
    # 如果 > 0，执行健康检查正常返回(exit 0)，则 priority = priority + weigtht，异常返回(exit 1)，则 priority 不变
    # priority只有在健康检查的返回状态发生改变时才会改变，例如：(weight -1，priority 100) 正常 -> 异常，priority = 100 - 1；异常 -> 正常，priority 恢复为100.
}

vrrp_instance VI_1 { # VRRP实例配置，一个keepalived可以配置多个VRRP实例，每个实例的virtual_router_id一定要不同
    state BACKUP # 实例初始状态（MASTER主，BACKUP备）
    nopreempt # 如果该实例从slave->Master就不能从Master->slave了，一般不设置
    interface em1 # 指定HA检测网络的接口
    virtual_router_id 10  # 虚拟路由标识，这个标识是一个数字(0-255)，同一个vrrp实例使用唯一的标识，即同一个vrrp_instance下的MASTER和BACKUP必须一致  
    priority 100 # MASTER权重要高于BACKUP，数字(0-255)越大优选级越高
    advert_int 1 # 组播信息发送间隔，两个节点设置必须一样
    authentication { # 主从服务器验证方式，设置验证类型和密码，MASTER和BACKUP必须使用相同的密码才能正常通信
        auth_type PASS
        auth_pass t9rveMP0Z9S1
    }
      track_script { 
        check_pg_alived # 执行定义的vrrp_script
     }
    virtual_ipaddress {
        192.168.1.26 # 虚拟IP 可以设置多个虚拟IP地址，每行一个（一般使用虚拟ip来访问）
    }
    smtp_alert # 如果vrrp_script异常退出(exit 1)则发送邮件
    notify_master /usr/local/bin/master.sh # 当当前vrrp实例从slave->Master时触发
    notify_backup /usr/local/bin/backup.sh # 当当前vrrp实例从master->slave时触发
    notify_fault /usr/local/bin/fault.sh # 当当前vrrp实例故障时触发
}
```

## 2. Master选举

Master选举最重要的原则是`priority`的大小，priority最大的节点将会抢占为Master。

具体的机制如下：
```
首先启动Master节点，然后启动backup节点。

MASTER其中一个职责是响应VIP的arp包，将VIP和mac地址映射关系告诉局域网内其他主机，同时，它还会以多播的形式（目的地址是标准的广播地址224.0.0.18）向局域网中发送VRRP通告，告知自己的优先级。

网络中的所有BACKUP节点只负责处理MASTER发出的多播包，当发现MASTER的优先级没自己高，或者没收到MASTER的VRRP通告时，BACKUP将自己切换到MASTER状态，然后做MASTER该做的事：1.响应arp包，2.发送VRRP通告。

在keepalived的运行过程中，会根据健康检查结果和weight的值动态调整优先级，再根据优先级的大小确定Master。当两个节点的优先级相同时，以节点发送VRRP通告的IP作为比较对象，IP较大者为MASTER。

考虑到有可能会有多个backup，如果所有的backup节点的初始优先级不同，则有可能会发生Master选举乱套了，所以所有backup节点的优先级一定要相同，并遵循公式：(MASTER priority - BAKCUP priority) < (weight)。

另外，当网络中不支持多播(例如某些云环境)，或者出现网络分区的情况，keepalived BACKUP节点收不到MASTER的VRRP通告，就会出现脑裂(split brain)现象，此时集群中会存在多个MASTER节点。
```