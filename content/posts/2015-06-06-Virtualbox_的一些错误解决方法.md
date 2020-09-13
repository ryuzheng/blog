---
title: "Virtualbox 的一些错误解决方法"
date: 2015-06-06T13:18:00+08:00
published: true
slug: virtualbox-de-yi-xie-cuo-wu-jie-jue-fang-fa
tags: ['代码']
cover_image: 
canonical_url: false
description: "这两天在研究Docker，尝试着在Windows上搭建，需要用到Virtualbox，本来我就比较喜欢Vmware，因为Virtualbox相对来说比较多错误，有时候还是比较烦的。"
---



这两天在研究Docker，尝试着在Windows上搭建，需要用到Virtualbox，本来我就比较喜欢Vmware，因为Virtualbox相对来说比较多错误，有时候还是比较烦的。

以下是我自己搭建时遇到的几个错误，同时也提供了我解决的方法。

##快要完成安装时发生倒退##

很不幸运地，从刚开始安装就发生了错误，我安装了很多次还是不行，上网搜索解决的方法，有人说不要勾选支持USB的选项，我尝试了也不行。

最后我是通过

>首先，如果你有360的软件，关闭360的Intel-VT核晶防护引擎，方法是打开360 -> 安全防护中心 -> 右上角的五角星 -> 关闭所谓的核晶防护引擎（估计360也是建一个虚拟机或者沙盒之类的来防御吧，但是很可恶地开了之后别的软件都无法使用Intel-VT，害得我的海马玩模拟器很卡）
然后，打开注册表 -> HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Network -> MaxNumFilters的值从8改成20 -> 重启再安装（这是我从贴吧找到的，亲测有效，但是Win10貌似不支持）

##虚拟机启动报错

[photos]
![](./images/dccc5010b912c8fc96fa6e73ff039245d78821ce.jpg)
![](./images/e957ae12c8fcc3ce95c9518d9145d688d53f20ce.jpg)
[/photos]

如果你的报错信息如图那样，还有类似以下的

>不能为虚拟电脑 boot2docker-vm 打开一个新任务.
The virtual machine 'boot2docker-vm' has terminated unexpectedly during startup with exit code 1 (0x1). More details may be available in 'C:\Users\Ericye\VirtualBox VMs\boot2docker-vm\Logs\VBoxStartup.log'.
返回 代码: E_FAIL (0x80004005) 
组件: Machine 
界面: IMachine {480cf695-2d8d-4256-9c7c-cce4184fa048} 

解决方法是

>因为vboxdrv服务没有安装或没有成功启动，
64位的系统经常这样，
找到安装目录下的vboxdrv文件夹，
如D:\Program Files\Oracle\VirtualBox\drivers\vboxdrv，
右击VBoxDrv.inf，选安装，然后重启。

##windows默认主题破解原因

有时Virtualbox的虚拟机创建不成功是因为你的电脑上的Windows默认主题被破解了，错误码一般有个1970。

![](./images/a94f3f381f30e92453b7c17048086e061d95f72c.jpg)

只需要还原主题就可以了，一般我们都是用*UniversalThemePatcher*来破解的，同样也可以用它来还原，只不过记得要下载有备份原本文件的版本，这里我提供一个版本以供使用，这个是有备份默认主题的。

![](./images/d043ad4bd11373f0625e540da40f4bfbfbed043a.jpg)

[七牛云](./images/[主题破解及还原工具].zip)

如果还有一些Virtualbox的错误的话，建议大家去看看[Virtualbox贴吧](http://tieba.baidu.com/f?kw=virtualbox&ie=utf-8)，上面有些主题提供了不错的解决方案。

-----
参考网页：
[win7安装最新版本4.3.20版本的VirtualBox，创建虚拟机启动报错](http://tieba.baidu.com/p/3495196597)