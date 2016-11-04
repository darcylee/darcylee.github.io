---
layout:     post
title:      "ibus使用过程中碰到的问题"
subtitle:   "ibus在debian8中不跟随，ibus-sunpinyin无法配置等问题"
date:       2016-11-4 14:19:30
header-img: "img/post-bg-unix.jpg"
author:     darcylee
tags:       [note, linux]

---

> 主要针对在`debian8` 和 `ubuntu-gnome-1604`linux系统使用过程中遇到的问题

# ibus-sunpiny 无法打开设置

下载python-ibus安装即可，依赖python-dbus软件包
先安装python-dbus

`sudo apt-get install python-dbus`

然后下载`python-ibus`,[下载地址](http://packages.ubuntu.com/search?suite=default&section=all&arch=any&keywords=python-ibus&searchon=names)



# 输入框跟随问题。

主要是因为ibus没有在GUI框架中注册

以下是傻瓜解决方法。安装几个包

`sudo apt-get install ibus-gtk ibus-gtk3 ibus-qt4`

将linux主流的GUI 框架都注册了，然后注销或者重启就搞定。
