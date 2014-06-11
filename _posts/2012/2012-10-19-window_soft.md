---
layout: post
title: window 软件
date: 2012-10-19
author: scsidisk
category: Windows
---

## 开发

- APMXE5 apache开发环境包
- BCompare-zh-3.2.3.13046 合并工具
- EditPlus-cn 编辑器
- Git-1.6.5.1-preview20091022 版本控制工具
- KDiff3Setup_0.9.95-2 文件比较工具
- Mercurial-1.4.1 版本控制工具
- Search & Replace汉化版 文本查找替换
- TortoiseGit-1[1].2.1.0-32bit git图形化工具
- npp-unicode 编辑器
- GoogleAppEngineLauncher-1[1].2.7.dmg
- HttpWatch.Professional.v5.2.16.rar
- IEDevToolBarSetup-IE开发工具.msi
- SSH Secure Shell-lite

## 本地化

- Language Localizator 6.rar
- eXeScope6.41.rar
- SDLPASSOLO2007
- reshack3.5_zh.rar
- SSH Secure Shell.rar

## 服务器

- IIS-back.vbs
- Serv-U.rar
- Symantec.AntiVirus.Corporate.v10.0.0.359.Final.CHS.rar
- epp312.exe
- mysql-noinstall-5.1.48-win32.zip
- php-5.2.13-Win32.zip
- phpMyAdmin-3.3.4-all-languages.zip
- vhost-t.rar
- wrar392.exe

## 系统工具

- 7z465 解压缩
- WinRAR 压缩解压
- Drive Rescue  1.9 文件恢复
- EasyRecoveryPro 文件恢复
- FinalDataNT15sd-LDR 不慎使用了Shift＋Del组合键删除了不该删除的文件
- VMware\_Workstation\_7\_CN 虚拟机
- ylpl\_VMware.Workstation\_v5.5.2\_small
- everest3 系统信息查看
- python-2.6.4 
- wubi 五笔输入法
- 易我磁盘医生注册版 文件恢复

## 办公文档

- MSOffice2007.zip
- Office Word 2003 绿色精简版.ZIP


## 浏览器

- ChromeSetup 浏览器
- Firefox Setup 3.5.5 
- SafariSetup
- Opera\_1010\_int\_Setup

## 上传下载

- FileZilla

## 通讯聊天

- ipmsg.zip

## 数据库

- MSSQL2000
- MSSQL2MySQLSync
- SQLyog
- mysql-connector-odbc-noinstall-5.1.5-win322

## 视频软件

- 2FLV beta 英文绿色免费版 是一个可以用来转换任何视频文件为flv格式的软件
- Video Converter
- flvcomb极速FLV合并器(FlvComb) V1.1 绿色版_将多个flv文件合并为一个.rar

## 光盘工具

- ONES 2.0.330
- UltraISO 9.3.6
- daemon300 虚拟光驱

## 其他

- PDFFoxitReader.rar
- PhpDocumentor-1.4.3.zip
- change-3389.reg
- findname域名管理.zip
- hex编辑器.rar
- p2pover207_home-1.rar
- pdg2pdf.rar

## 修改3389端口

保存为change-3389.reg

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp]
"PortNumber"=dword:0000cfeb

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]
"PortNumber"=dword:0000cfeb

[HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\SharedAccess\Parameters\FirewallPolicy\StandardProfile\GloballyOpenPorts\List]
"53227:TCP"="53227:TCP:*:Enabled:rpd-53227-tcp"
"53227:UDP"="53227:UDP:*:Enabled:rdp-53227-udp"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\StandardProfile\GloballyOpenPorts\List]
"53227:TCP"="53227:TCP:*:Enabled:rpd-53227-tcp"
"53227:UDP"="53227:UDP:*:Enabled:rdp-53227-udp"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\StandardProfile\GloballyOpenPorts\List]
"53227:TCP"="53227:TCP:*:Enabled:rpd-53227-tcp"
"53227:UDP"="53227:UDP:*:Enabled:rdp-53227-udp"
```




