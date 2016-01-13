---
layout: page
title: about
description: ""
permalink: /about/
slug: about
---

# MSVE-2015-070601漏洞分析

`MSVE-2015-070601`是一次完整的针对主机层面的漏洞渗透，外部表现为利用被入侵主机，植入rootkit程序使其沦陷为肉鸡，对外提供监听服务并尝试主动向外部网络发送大量IP包，导致互联网出口防火墙拥塞负载升高，处理能力下降，从而导致办公网正常出口流量受限。同时通过提权漏洞导致获得root权限。

## 分析
对主机22.78进行`netstat -anp`

发现异常UDP监听端口项目：

![Alt text](./1436230280022.png)

查看/proc 详细信息：`ls -al /proc/20962/`
![Alt text](./1436230335390.png)

发现实际可执行文件指向的是：`/usr/bin/gtccpsnfah`
`-rwxr-xr-x 1 root root 625740 07-06 17:04 /usr/bin/gtccpsnfah`

初步判断此文件是发起大量TCP请求的后门程序。

检查crontab：

![Alt text](./1436230512214.png)

发现一条每三分钟的恶意脚本，每三分钟会变换一次后面进程的名字同时重新编译生成/usr/bin下的二进制文件

同时在系统启动项发现所有runlevel的自启动的项目：
![Alt text](./1436230767035.png)




## 修复
备份业务文件，回收主机，同时监控同网络其他主机活动情况，定期进行rootkit检查。

