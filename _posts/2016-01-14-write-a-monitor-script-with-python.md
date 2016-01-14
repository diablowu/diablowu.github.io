---
layout: post
title: "用python实现简单的监控脚本"
permalink: write-a-monitor-script-with-python
date: 2016-01-14 11:46:13
comments: true
description: "write a monitor script with python"
keywords: ""
categories: tech
tags:
- python
---

{% highlight python %}
#!/usr/bin/env python
import commands
import sys
from string import strip
import datetime

_cli_pre = 'ssh weixin@%s -C "%s"'

_scp_logs = 'scp weixin@%s%s %s'

_delta = datetime.timedelta(days=-1)
_yst = datetime.date.today() + _delta
_logfile = '/data/var/logs/%s/%s/80_access_%s.log' % (_yst.strftime('%Y'),_yst.strftime('%m'),_yst.strftime('%Y%m%d'))




sc = [
    'wm1':{
        'ip':'10.6.97.46',
        'splunk':'10.6.10.186'
    },
    'wm3':{
        'ip':'10.6.97.50',
        'splunk':'10.6.10.186'
    },
    'wm4':{
        'ip':'10.6.97.51',
        'splunk':'10.6.10.186'
    },
    'wm5':{
        'ip':'10.6.97.56',
        'splunk':'10.6.10.186'
    },
    'wm6':{
        'ip':'10.6.97.57',
        'splunk':'10.6.10.186'
    },
    'wm7':{
        'ip':'10.6.97.58',
        'splunk':'10.6.10.186'
    },
    'wm8':{
        'ip':'10.6.97.59',
        'splunk':'10.6.10.186'
    },
    'wm9':{
        'ip':'10.6.97.60',
        'splunk':'10.6.10.186'
    },
    'wm10':{
        'ip':'10.6.97.61',
        'splunk':'10.6.10.186'
    },
    'wm11':{
        'ip':'10.6.97.62',
        'splunk':'10.6.10.186'
    },
    'wm12':{
        'ip':'10.6.97.63',
        'splunk':'10.6.10.186'
    },
    'wm13':{
        'ip':'10.6.97.64',
        'splunk':'10.6.10.186'
    },
    'wm14':{
        'ip':'10.6.97.65',
        'splunk':'10.6.10.186'
    },
                                                
]

"""
#wm host
10.6.97.46  wm1
10.6.97.50  wm3
10.6.97.51  wm4
10.6.97.56  wm5
10.6.97.57  wm6
10.6.97.58  wm7
10.6.97.59  wm8
10.6.97.60  wm9
10.6.97.61  wm10
10.6.97.62  wm11
10.6.97.63  wm12
10.6.97.64  wm13
10.6.97.65  wm14
"""







def execmd(cmd,host):
    status,output = commands.getstatusoutput(cmd)
    print 'host : %s' % host
    print  output
    print  '\n'



def getlogs(host,ip,srclogfile,dstlogfile):
    print host , ip ,srclogfile ,dstlogfile

    
def sendlogs(dstlogfile,splunkserver):
    print dstlogfile


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