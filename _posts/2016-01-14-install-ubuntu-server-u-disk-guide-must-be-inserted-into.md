---
layout: post
title: "u盘安装ubuntu-server必须插入u盘引导的解决办法"
permalink: install-ubuntu-server-u-disk-guide-must-be-inserted-into
date: 2016-01-14 17:45:33
comments: false
description: "install ubuntu-server U disk guide must be inserted into"
keywords: ""
categories: tech
tags:
- ubuntu
- linux
- grub
---

想把已经落灰的老笔记本改造成下载机和nas使用，第一步要装个ubuntuserver，因为光驱位已经改成二硬盘，所以使用usb安装，很简单，使用Universal-USB-Installer就可以，一切按照安装步骤完成后最后安装grub到硬盘上，一切完成后发现如果拔掉u盘，只能进入grub引导界面，不能进入系统，怀疑是安装过程中把grub写入到了u盘，于是插入u盘再次开机，发现果然可以启动。
<!--more-->

解决办法：

插入u盘进入系统，重新安装grub：

{% highlight bash %}
$sudo -i

#grub-install /dev/sda

#update-grub

#reboot

{% endhighlight %}
