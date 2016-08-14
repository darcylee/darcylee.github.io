---
layout:     post
title:      "zephyr 项目在macos平台的开发"
subtitle:   "QT"
date:       2016-8-14 12:19:30
header-img: "img/post-bg-default.jpg"
author:     darcylee
tags:       [学习笔记, zephyr]

---

## 关于`zephyr™`项目

[zephyr™](https://www.zephyrproject.org), 首先它是一个开源的项目，主要目的是针对资源有限的系统设计的实时的系统内核，可以胜任从简单的嵌入式传感器到复杂的物联网网关等应用场景。官方的文档也相对比较完整，可以[移步官网了解一下](https://www.zephyrproject.org/doc/)。
该项目开源于今天2月份，到目前版本已升级到1.5.0，使用git可以clone整个仓库

```
git clone https://gerrit.zephyrproject.org/r/zephyr
```
zephyr™使用类似linux内核`Kbuild`构建系统来组织整个项目的，相信有一定嵌入式linux开发经验的童鞋们，看了官网的[QuickStart](https://www.zephyrproject.org/doc/getting_started/getting_started.html)文档后，很容易上手开发的， 文档中提供了三个不同平台`linux`、`windows`、`macOS`的开发环境设置介绍，如果你目前使用的是`linux`或者`windows`平台可以绕道了，因为接下去我要说的是基于`macOS`平台的开发环境设置。

虽然官网有给出`macOS`平台的[开发环境设置指南](https://www.zephyrproject.org/doc/getting_started/installation_mac.html)，但是还是会碰到一些问题，我这里记录了一下我的设置过程，以及我碰到的问题的解决办法，希望能帮到和我一样想在`macOS`平台上做基于`zephyr`的嵌入式开发的朋友。

## 开发环境设置

* 首先请把系统设计到`Mac OS X 10.11 (El Capitan)`，我是基于这个版本来设置的。

#### 安装依赖包

安装`Homebrew`， 安装过的同学跳过， `Homebrew`好比debian系Linux系统里的`apt-get`工具，很好很强大。安装很简单，移步`Homebrew`[官网](http://brew.sh)，按照上面的说明安装。

继续安装一些工具

```
brew install gettext qemu help2man mpfr gmp coreutils wget python3

brew tap homebrew/dupes

brew install grep --with-default-names

pip3 install ply

```

安装`Crosstool-ng`

```
brew install crosstool-ng
```

#### 编译工具链

编译工具链需要一个大小写敏感的分区，使用`macOS`自带的`diskutil`可以很方便的创建一个出来。创建一个8G左右的分区
![img](img/art-post/post_zephyr_on_mac/creat_case-sensitive.png)


