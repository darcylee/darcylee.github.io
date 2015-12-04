---
layout:     post
title:      "更新记录"
subtitle:   "远传下位机软件"
date:       2015-12-04 21:10:30 +0800
author:     darcylee
published: false
tags: [objective-c, 学习笔记]
---

### 文件树
```
--/path/sryc_software...
  |--BINs
  |   |--boot.bin           <----boot程序
  |   |--sryc_dtu.bin       <----远传应用程序程序
  |   |--boot+sryc_dtu.bin  <----整个flash文件，可用来空片下载
  |--Tools
      |--SmartDownload.exe
      |--ToolKit-setting.exe
      |--ToolKit-upload.exe
```
1. boot.bin 就是boot程序
2. sry_dtu.bin 为远传的应用程序，不可以作为jflash直接下载使用，必须配合PC工具 “smartdownload.exe”来下载，使用前请确保boot已经下载进芯片。
3. boot+sryc_dtu.bin 为boot 和应用程序的拼接文件，可以直接由jflash下载在芯片的0x0地址上，下载完成后，重新上电即可正常运行。

### 修改记录

* **2015年12月2日**
1. `sryc_dtu`压力字段名称修改为`"press"`,版本升级到`EM-SRYC-V1.2.2`
1. pc工具`ToolKit-upload.exe` 新增离线数据查看功能，版本升级为`v1.2.0` 。
2. pc工具`ToolKit-upload.exe` 压力字段名称同步修改为`"press"`。

* **2015年11月25日**
1. 上传服务器的采集时间精确到秒，解决服务器端显示采集时间为1976的bug。
2. pc工具`ToolKit-upload.exe`新增上传的采集时间精确到秒，版本升级为`v1.1` 。

* **2015年11月20日**
1. 增加压力读取。
2. pc工具`ToolKit-setting.exe`版本升级到v1.1.0，适配新增的压力字段显示。
3. 新版本水表数据读取还会偶尔出现失败，因有失败重读机制，无太大问题，后续慢慢解决。

* **2015年10月21日**
1. 修复休眠无法唤醒的bug 。
2. 此次发布的bin文件，作为剩下2台样机的生产文件,软件版本升级到v1.2 。