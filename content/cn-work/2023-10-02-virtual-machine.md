---
title: 网络共享
date: '2023-10-02'
slug: virtual-machine
---

桥接：相当于把虚拟机当作局域网中一台独立的设备，局域网中的其它设备也能连接到。
仅主机：就是只和主机单独通信。
NAT：相当于在主机里建立一个虚拟路由器，虚拟机连接这个虚拟路由器。

virtual player设置网络模式为桥接模式，v2ray中开启允许局域网连接。

物理主机中查看Ipv4地址

```powershell
ipconfi 
```

v2ray中查看http, https, socket查看端口号。

虚拟主机设置中代理填入对应的ip以及端口号。
