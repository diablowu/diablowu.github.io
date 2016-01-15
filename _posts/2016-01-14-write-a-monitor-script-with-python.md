---
layout: post
title: "用python实现简单的监控脚本"
permalink: write-a-monitor-script-with-python
date: 2016-01-14 11:46:13
comments: false
description: "write a monitor script with python"
keywords: ""
categories: tech
tags:
- python
- monitor
- sysadmin
---

{% highlight python %}
#!/usr/bin/env python
import commands
import sys
from string import strip
import datetime

if __name__ == '__main__':
    for k in sc:
        host = k
        splunkserver = sc[k]['splunk']
        ip = sc[k]['ip']
        dstlog = '/data/var/logs/'

        locallog = '/data/var/logs/%s_%s_access.log' % (ip,_yst.strftime('%Y%m%d'))
        getlogs(k,ip,_logfile,locallog)
    #for host in servers:
    #    execmd(_cli_pre % (host,cmd),host)


{% endhighlight %}