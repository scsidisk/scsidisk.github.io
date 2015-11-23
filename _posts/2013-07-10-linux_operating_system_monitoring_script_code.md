---
layout: post
title: 几个常用的Linux操作系统监控脚本代码
date: 2013-04-10
author: scsidisk
category: Linux
tags: [CentOS, Linux, Bash]
---

本文介绍了几个常用的Linux监控脚本，可以实现主机网卡流量、系统状况、主机磁盘空间、CPU和内存的使用情况等方面的自动监控与报警。根据自己的需求写出的shell脚本更能满足需求，更能细化主机监控的全面性

本文介绍了几个常用的Linux监控脚本，可以实现主机网卡流量、系统状况、主机磁盘空间、CPU和内存的使用情况等方面的自动监控与报警。根据自己的需求写出的shell脚本更能满足需求，更能细化主机监控的全面性。

最近时不时有互联网的朋友问我关于服务器监控方面的问题，问常用的服务器监控除了用开源软件，比如：cacti，nagios监控外是否可以自己写shell脚本呢?根据自己的需求写出的shell脚本更能满足需求，更能细化主机监控的全面性。

下面是我常用的几个主机监控的脚本，大家可以根据自己的情况再进行修改，希望能给大家一点帮助。

### 1、查看主机网卡流量

```bash
#!/bin/bash
#network
#Mike.Xu
while : ; do
	time='date +%m"-"%d" "%k":"%M'
	day='date +%m"-"%d'
	rx_before='ifconfig eth0|sed -n "8"p|awk '{print \$2}'|cut -c7-'
	tx_before='ifconfig eth0|sed -n "8"p|awk '{print \$6}'|cut -c7-'
	sleep 2
	rx_after='ifconfig eth0|sed -n "8"p|awk '{print \$2}'|cut -c7-'
	tx_after='ifconfig eth0|sed -n "8"p|awk '{print \$6}'|cut -c7-'
	rx_result=\$[(rx_after-rx_before)/256]
	tx_result=\$[(tx_after-tx_before)/256]
	echo "\$time Now_In_Speed: "\$rx_result"kbps Now_OUt_Speed: "\$tx_result"kbps"
	sleep 2
done
```


### 2、系统状况监控

```bash
#!/bin/sh
#systemstat.sh
#Mike.Xu
IP=192.168.1.227
top -n 2| grep "Cpu" >>./temp/cpu.txt
free -m | grep "Mem" >> ./temp/mem.txt
df -k | grep "sda1" >> ./temp/drive_sda1.txt
#df -k | grep sda2 >> ./temp/drive_sda2.txt
df -k | grep "/mnt/storage_0" >> ./temp/mnt_storage_0.txt
df -k | grep "/mnt/storage_pic" >> ./temp/mnt_storage_pic.txt
time=`date +%m"."%d" "%k":"%M`
connect=`netstat -na | grep "219.238.148.30:80" | wc -l`
echo "\$time \$connect" >> ./temp/connect_count.txt
```

### 3、监控主机的磁盘空间，当使用空间超过90%就通过发mail来发警告

```bash
#!/bin/bash
#monitor available disk space
SPACE='df | sed -n '/ \ / \$ / p' | gawk '{print \$5}' | sed 's/%//'
if [ \$SPACE -ge 90 ] then
	fty89@163.com
fi
```

### 4、 监控CPU和内存的使用情况

```bash
#!/bin/bash
#script to capture system statistics
OUTFILE=/home/xu/capstats.csv
DATE='date +%m/%d/%Y'
TIME='date +%k:%m:%s'
TIMEOUT='uptime'
VMOUT='vmstat 1 2'
USERS='echo \$TIMEOUT | gawk '{print \$4}' '
LOAD='echo \$TIMEOUT | gawk '{print \$9}' | sed "s/,//' '
FREE='echo \$VMOUT | sed -n '/[0-9]/p' | sed -n '2p' | gawk '{print \$4} ' '
IDLE='echo \$VMOUT | sed -n '/[0-9]/p' | sed -n '2p' |gawk '{print \$15}' '
echo "\$DATE,\$TIME,\$USERS,\$LOAD,\$FREE,\$IDLE" >> \$OUTFILE
```

### 5、全方位监控主机

```bash
#!/bin/bash
# check_xu.sh # 0 * * * * /home/check_xu.sh
DAT="`date +%Y%m%d`"
HOUR="`date +%H`"
DIR="/home/oslog/host_\${DAT}/\${HOUR}"
DELAY=60 COUNT=60
# whether the responsible directory exist
if ! test -d \${DIR} then
	/bin/mkdir -p \${DIR}
fi
# general check
export TERM=linux
/usr/bin/top -b -d \${DELAY} -n \${COUNT} > \${DIR}/top_\${DAT}.log 2>&1 &
# cpu check
/usr/bin/sar -u \${DELAY} \${COUNT} > \${DIR}/cpu_\${DAT}.log 2>&1 &
#/usr/bin/mpstat -P 0 \${DELAY} \${COUNT} > \${DIR}/cpu_0_\${DAT}.log 2>&1 &
#/usr/bin/mpstat -P 1 \${DELAY} \${COUNT} > \${DIR}/cpu_1_\${DAT}.log 2>&1 &
# memory check
/usr/bin/vmstat \${DELAY} \${COUNT} > \${DIR}/vmstat_\${DAT}.log 2>&1 &
# I/O check
/usr/bin/iostat \${DELAY} \${COUNT} > \${DIR}/iostat_\${DAT}.log 2>&1 &
# network check
/usr/bin/sar -n DEV \${DELAY} \${COUNT} > \${DIR}/net_\${DAT}.log 2>&1 &
#/usr/bin/sar -n EDEV \${DELAY} \${COUNT} > \${DIR}/net_edev_\${DAT}.log 2>&1 &
```

放在crontab里每小时自动执行：

```
0 * * * * /home/check_xu.sh
```

这样会在/home/oslog/host_yyyymmdd/hh目录下生成各小时cpu、内存、网络，IO的统计数据。

如果某个时间段产生问题了，就可以去看对应的日志信息，看看当时的主机性能如何。