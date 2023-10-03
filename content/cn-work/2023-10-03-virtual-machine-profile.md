---
title: 虚拟机配置
date: 2023-10-03
slug: virtual machine profile
---

这[这里](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)下载vmware playrer，[这里](https://ubuntu.com/download/desktop)下载ubuntu镜像。打开vmware，按照提示一路选择操作既可。

**虚拟主机共享主机v2ray代理**

vmware player设置网络模式为NAT模式。

物理主机中运行`ipcongfig`查看WLAN ipv4地址。v2ray中开启允许局域网连接，查看局域网socket, http端口号。
 
```cmd
ipconfig
```

虚拟主机中在 Settings -> Network -> Network Proxy中设置为mannual在http, https前一栏中填入物理主机的ipv4地址，后一栏中填入v2ray中查看的http端口号。在socket的第二栏中填入v2ray中查看的socket端口号。

**中文输入设置**

> [From network](https://askubuntu.com/questions/1408873/ubuntu-22-04-chinese-simplified-pinyin-input-support)
- Open Settings, go to Region & Language -> Manage Installed Languages -> Install / Remove languages.
- Select Chinese (Simplified). Make sure Keyboard Input method system has Ibus selected. Apply.
- Reboot
- Log back in, reopen Settings, go to Keyboard.
- Click on the "+" sign under Input sources.
- Select Chinese (China) and then Chinese (Intelligent Pinyin).


**外观**

高分辨率屏幕下ubuntu的字体太小，可以下载gnome-tweaks工具对字体大小，缩放，外观进行小的调整。

```bash
sudo apt-get install gnome-tweaks
```
下载完成以后在show application中搜索tweaks打开，进行修字体，外观等。

**软件下载**

对于必要的软件通过包管理工具apt 或者 sanp下载。非必要的软件建议使用conda管理，若codna中没有，则建议使用压缩包解压。

**R配置**

使用vscode作为代码编辑器，直接参考[vscode-R文档](https://github.com/REditorSupport/vscode-R/wiki/Getting-Started)配置即可。

其它的编程语言同样直接参考[vscode documentation](https://code.visualstudio.com/docs)

vscode 可以直接使用 snap 下载

```bash
snap install code
```
