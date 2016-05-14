---
title: ARP 欺骗
date: 2016-05-14 14:30:16
tags:
 - Security
 - Network
categories:
 - Security.Network
---
LAN里面的客户机和路由器共用一个和外部分离的IP地址空间，那个子网掩码就表示哪些位是固定的。

LAN里面的机器通讯是要MAC地址（每个网卡唯一）的。如果要发到外网里面，MAC地址就填路由器的地址，否则填真的MAC地址。

基于通讯协议的某些奇怪的特性，每个机器只知道对方的IP地址，然后维护了一个ARP表来查询MAC地址。

而且得到这个ARP表格的方式非常简单，就是在LAN里面广播问一下，然后对应IP地址的机器就会回应。然后机器不会检查那个回应的有效性，直接就修改表格了。

所谓ARP欺骗就是自己发一个回应把其他机器的ARP表格都修改了。这样所以LAN里面的通讯都会经过自己。然后就可以用Wireshark来监听了。

不用自己操作这些过程，可以用Ettercap。

![点击查看Ettercap教程](/attachments/ARP 欺骗/1.pdf)
