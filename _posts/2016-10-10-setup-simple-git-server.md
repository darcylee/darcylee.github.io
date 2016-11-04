---
layout:     post
title:      "搭建简单的git服务器"
subtitle:   "在数梅派上搭建简单git服务器"
date:       2016-10-10 10:19:30
header-img: "img/post-bg-rpi.jpg"
author:     darcylee
tags:       [linux, git, RaspberryPi]

---


## 安装`git`

> sudo apt-get install git

## 创建一个`git`用户和组

创建一个`git`组

> sudo groupadd git

创建一个`git`用户，指定家目录`/home/git`, 指定登入`shell`为git-shell,禁止`git`用户以`bash-shell`登入服务器,并加入用户组`git`

> sudo mkdir /home/git

> sudo chown git:git /home/git

> sudo useradd -d /home/git -m git -s /usr/bin/git-shell -G git

## 创建证书登入

收集所有需要登入的用户公钥, 就是用户的`id_rsa.pub`文件,把公钥导入到`/home/git/.ssh/authorized_keys`

如果没有`authorized_keys`文件则创建

> touch /home/git/.ssh/authorized_keys

然后导入需要访问该服务器git仓库的用户的ssh公钥， 可以手动导入

> cat id_rsa.pub >> /home/git/.ssh/authorized_keys

如果觉得手动导入比较麻烦，可以使用下面方法导入

> ssh-copy-id git@server[^1]

##  创建一个仓库

在`/srv`下创建一个git仓库

> cd /srv

> sudo git init --bare test.git


修改属性,设置只允许`git`用户访问

> sudo chown git:git -R test.git

## 克隆远程仓库

```
git clone git@server:/srv/test.git
Cloning into 'test'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
```

到此`git`服务器搭建完成

## NOTE

[^1]: `server`请换成你的树梅派`ip`地址或者域名
