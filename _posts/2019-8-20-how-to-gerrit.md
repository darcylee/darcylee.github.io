---
layout:         post
title:          "How To Gerrit"
header-style:   "text"
date:           2019-8-20
tags:           [gerrit, git]

---

# Gerrit
## 角色概念

| 用户名 | 角色       | 说明                                         |
| 张三   | 代码提交人 | 准备提交代码，触发评审任务之人               |
| 李四   | 同行评审   | 技术组内同行，评审代码之人，可以不止一个     |
| 王五   | 提交确认   | 一般是仓库负责人，确认代码评审完成，可以入库 |

# Gerrit使用前期准备
## 添加ssh key

将自己的公钥上传到 gerrit 系统。（密钥对一般在 `~/.ssh` 目录 ）

```
cat ~/.ssh/id_rsa.pub
```

上传公钥到gerrit系统

`gerrit系统主页` -> `用户名称` -> `Settings` -> `SSH Keys` 将公钥黏贴到 `New SSH Key` 中，最后点  `ADD NEW SSH KEY` 将公钥添加到gerrit系统。

> 请确认当前的 `~/.ssh/id_rsa.pub` 是自己的公钥

如果 `~/.ssh` 目录下没有公钥，使用命令 `ssh-keygen` 生成

``` bash
ssh-keygen -b 4096 -t rsa -C your_mail_name@gmail.com
```

默认公钥名为id_rsa.pub，可以使用 `-f` 参数产生指定名称的密钥对。

当使用非标准路径（标准路径为 `~/.ssh` ）的密钥对时，需要手动指定私钥的路径，可以新建一个文件 `~/.ssh/config` ，将以下内容加入该文件

``` bash
Host gerrit.xxx.work
Port 29418
IdentityFile  /path/of/your/private_sshkey
```

## 验证key添加成功
使用命令 `ssh -p 29418 your_mail_name@gerrit.xxx.work` ，出现如下信息，则表示添加成功

``` text
ssh -p 29418 your_mail_name@gerrit.xxx.work

####    Welcome to Gerrit Code Review    ####

Hi your_mail_name, you have successfully connected over SSH.

Unfortunately, interactive shells are disabled.
To clone a hosted Git repository, use:

git clone ssh://lijiaquan@gerrit.xxx.work:29418/REPOSITORY_NAME.git

Connection to gerrit.xxx.work closed.
```

## 创建评审项目

这个一般由项目owner或者gerrit管理员完成，使用者无需太关心

## 项目权限配置

目前配置权限必须在 `Old UI` 下面才可以配置，可以点底部 `Switch to Old UI` 进行切换

配置说明...

# 发起一次 Review
## 下载gerrit仓库代码

必须使用 `Clone with commit-msg hook` 下的链接才能用

## 修订问题

- 正常修订代码，本地编译，完成验证等工作
- git add
- git commit （这里需要先到CAF系统上申请一个CAF表单号， commit message 必须带有CAF表单）

## 推送到gerrit
本地修订并add/commit完成后，使用下面的命令，将修订推送到gerrit服务器发起评审

    git push origin HEAD:refs/for/${branch_name}

为方便后面的push操作，可以使用下面的命令，设置推送关联，设置完后面可以直接使用 `git push` 命令推送到正确的分支

    git config remote.origin.push "refs/heads/*:refs/for/*"

## 开始评审流程

1.  `ADD REVIEWER` 添加评审人员
  在详细评审页面左边，点击 `ADD REVIEWER` 弹出菜单后，可以输入需要参与评审的人员邮件，选好后点 `SEND` 相关人员将收到一个评审请求邮件

2. `DIFF PREFERENCES` 查看本次的修订，进行review
3. `REPLY` 评审打分
4. `CODE REVIEW +2` 直接允许提交（有的权限人才会显示）
5. `SUBMIT` 评审通过后提交合并(代码变更会自动同步svn仓库)
6. `ABANDON` 放弃评审
7. `Code-Review` `Verified` 概念介绍
8. 可以使用 "?" 查看支持的快捷键说明

## patch评审不通过被打回

1. 本地修改后验证ok
2. 使用 `--amend` 参数进行commit <``` #很重要#
3. `git push origin HEAD:refs/for/${branch_name}` 再次推送到gerrit

> 当遇到多次修订同一个REVIEW时，第二次开始的commit必须使用 `--amend` 参数，否则push后将产生2个不同的评审单

reviewer 可以选择只看某次修改的点，这样可以缩小评审范围。

## 下载到本地review

`Review 页面` -> `MORE` -> `Download patch`

不同选项的含义

1. `Checkout`: 拉取patch，并基于 FETCH_HEAD 签出一个独立的分支
2. `Cherry Pick`: 拉取patch，并合并到当前分支上
3. `Format Patch`: 拉取patch，并产生一个单独的patch文件
4. `Pull`: 直接将修改merge到本地仓库

一般情况下，建议使用 `Cherry Pick` 来拉取，这样可以用 `git reset --hard HEAD~1` 将本地工作仓库恢复成原来的样子

# 工程实践
- vscode 插件
  
  **Gerrit**: <https://marketplace.visualstudio.com/items?itemName=tht13.gerrit-vscode>
  
  **Gerrit View**: <https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.gerrit-view>

- emacs 插件

  <https://github.com/darcylee/magit-gerrit>

- TortoiseGit上的gerrit使用
