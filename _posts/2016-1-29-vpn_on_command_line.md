---
layout:     post
title:      "命令行连接VPN"
subtitle:   "using VPN on the command line"
date:       2016-1-29 10:19:30 +0800
header-img: "img/post-bg-debian.jpg"
author:     darcylee
tags:       [linux, shell]

---

`VPN` 相信大家都很熟悉，在有图形界面的系统上，使用`VPN`我想大家都不会特别困难。比如在`windows`，或者有图形界面的`linux`，甚至在手上，设置都相当容易。然而要在没有图形界面的系统上使用`VPN`呢？大家可能都没那么熟悉了，今天就来趴一趴如何在字符终端上建立`VPN`连接。

#### 1. 安装`VPN`客户端

```bash
dl@debian ~/work  [Fri 29 Jan 2016 09:48:55 AM CST]
$ sudo apt-get install pptp-linux 
```

#### 2. 创建`VPN`连接

假定：你需要拨号的vpn服务器地址为`44.125.4.11`，你从`vpn`服务提供商那里申请的帐号为`abc`，密码为`123456`，你么你可以这样创建一个名为`myVpnName `的`vpn`连接

```bash
sudo pptpsetup --create myVpnName --server 44.125.4.11 --username abc --password 123456 --encrypt --start
```

- \-\-create 后的是创建的连接名称，可以为任意名称;
- \-\-server 后接的是vpn服务器的IP;
- \-\-username 是用户名
- \-\-password 是密码，在这也可以没这个参数，命令稍后会自动询问。这样可以保证账号安全
- \-\-encrypt 是表示需要加密，不必指定加密方式，命令会读取配置文件中的加密方式
- \-\-start 是表示创建连接完后马上连接，如果你不想连，就不写

如果没有出现意外，你在控制台中将看到如下输出，说明你的vpn已经建立成功，并且以成功连接到`VPN`服务器

```bash
Using interface ppp0
Connect: ppp0 <--> /dev/pts/1
CHAP authentication succeeded
local  IP address 192.168.99.12
remote IP address 192.168.99.1
```
我们查看下本地IP配置，发现多了一个ppp0的连接

```bash
dl@debian ~/work  [Fri 29 Jan 2016 09:52:58 AM CST]
$ ifconfig 
eth0      ....

lo        ....

ppp0      Link encap:Point-to-Point Protocol  
          inet addr:192.168.99.12  P-t-P:192.168.99.1  Mask:255.255.255.255
          UP POINTOPOINT RUNNING NOARP MULTICAST  MTU:1496  Metric:1
          RX packets:6 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:3 
          RX bytes:60 (60.0 B)  TX bytes:66 (66.0 B)
```


#### 3. 常用操作

* 打开`VPN`
`sudo pon myVpnName `<-`myVpnName`就是上面创建的vpn名称

* 关闭`VPN`
`sudo poff myVpnName`。如果你再次`ifconfig`查看本地连接，发现刚才出现的`ppp0`连接已经不见了。

#### 4. 全部流量走`vpn`通道

我们分别`ping`下`google`和`baidu`服务器

```bash
dl@debian ~/work  [Fri 29 Jan 2016 09:52:15 AM CST]
$ ping www.google.com
PING www.google.com (216.58.221.68) 56(84) bytes of data.
^C
--- www.google.com ping statistics ---
14 packets transmitted, 0 received, 100% packet loss, time 13000ms

dl@debian ~/work  [Fri 29 Jan 2016 09:53:01 AM CST]
$ ping www.baidu.com
PING www.a.shifen.com (14.215.177.38) 56(84) bytes of data.
64 bytes from 14.215.177.38: icmp_seq=1 ttl=128 time=18.6 ms
64 bytes from 14.215.177.38: icmp_seq=2 ttl=128 time=19.0 ms
64 bytes from 14.215.177.38: icmp_seq=3 ttl=128 time=18.9 ms
```
发现无法ping通墙外的`google`而国内的`baidu`正常，细心的你一定发现了，`baidu`的连接延时和没有拨`VPN`时是相近的。这是为什么？原因是我们新创建的`VPN`通道并没有被加入系统路由，可以`route`
查看一下。

```bash
dl@debian ~  [Fri 29 Jan 2016 11:27:39 AM CST]
$ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         10.211.55.1     0.0.0.0         UG    0      0        0 eth0
10.211.55.0     *               255.255.255.0   U     0      0        0 eth0
li1100-244.memb 10.211.55.1     255.255.255.255 UGH   0      0        0 eth0
link-local      *               255.255.0.0     U     1000   0        0 eth0
192.168.99.1    *               255.255.255.255 UH    0      0        0 ppp0
```
你看`default`后面`Iface`是`eth0`并不是我们创建的`ppp0`，我们需要手动创建：

```
dl@debian ~  [Fri 29 Jan 2016 11:32:00 AM CST]
$ sudo route add default dev ppp0
```
再次`route`

```bash
dl@debian ~  [Fri 29 Jan 2016 11:32:48 AM CST]
$ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         *               0.0.0.0         U     0      0        0 ppp0
default         10.211.55.1     0.0.0.0         UG    0      0        0 eth0
10.211.55.0     *               255.255.255.0   U     0      0        0 eth0
li1100-244.memb 10.211.55.1     255.255.255.255 UGH   0      0        0 eth0
link-local      *               255.255.0.0     U     1000   0        0 eth0
192.168.99.1    *               255.255.255.255 UH    0      0        0 ppp0
```

出现了2条`default`，其中一条是我们创建的痛过`ppp0`通道的，我们再次`ping`一下，发现都可以通了。

```bash
dl@debian /etc/ppp/peers  [Fri 29 Jan 2016 11:33:09 AM CST]
$ ping www.google.com
PING www.google.com (216.58.221.228) 56(84) bytes of data.
64 bytes from hkg07s21-in-f4.1e100.net (216.58.221.228): icmp_seq=1 ttl=53 time=493 ms
64 bytes from hkg07s21-in-f4.1e100.net (216.58.221.228): icmp_seq=2 ttl=53 time=440 ms
64 bytes from hkg07s21-in-f4.1e100.net (216.58.221.228): icmp_seq=3 ttl=53 time=436 ms
64 bytes from hkg07s21-in-f4.1e100.net (216.58.221.228): icmp_seq=4 ttl=53 time=440 ms
64 bytes from hkg07s21-in-f4.1e100.net (216.58.221.228): icmp_seq=5 ttl=53 time=440 ms
64 bytes from hkg07s21-in-f4.1e100.net (216.58.221.228): icmp_seq=6 ttl=53 time=444 ms
^C
--- www.google.com ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 9511ms
rtt min/avg/max/mdev = 436.317/449.187/493.006/19.749 ms
dl@debian /etc/ppp/peers  [Fri 29 Jan 2016 11:32:54 AM CST]
$ ping www.baidu.com
PING www.a.shifen.com (14.215.177.37) 56(84) bytes of data.
64 bytes from 14.215.177.37: icmp_seq=2 ttl=45 time=525 ms
64 bytes from 14.215.177.37: icmp_seq=3 ttl=45 time=570 ms
64 bytes from 14.215.177.37: icmp_seq=4 ttl=45 time=574 ms
64 bytes from 14.215.177.37: icmp_seq=5 ttl=45 time=567 ms
^C
--- www.a.shifen.com ping statistics ---
5 packets transmitted, 4 received, 20% packet loss, time 4001ms
rtt min/avg/max/mdev = 525.227/559.586/574.565/19.994 ms

```

如果你还不能确定新创建的`VPN`已正常使用，可以通过下面命令来查看外网的`IP`

```bash
dl@debian /etc/ppp/peers  [Fri 29 Jan 2016 10:31:12 AM CST]
$ curl ifconfig.me
44.125.4.11 ＃这个其实就是你vpn服务器的ip地址，这里只是个举例
```

至于如何自动完成路由的添加，目前还没有试验出来。等发现了添加说明。


