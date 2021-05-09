---
layout:         post
title:          "升级debian11(bullseye)"
subtitle:       "从debian10 升级到 debian11 优化记录"
header-style:   "text"
date:           2021-2-22
tags:           [debian, linux]

---

当前 2021/3/22 debian11(bullseye) 暂未最终发布，但已经到了软件冻结的阶段，软件版本和特性都已经完全冻结，是时候
升级一下自己用了2年的 debian10(buster) 了，这里记录一下整个升级和优化过程。

# 升级方法

当前还未发布完整的安装镜像，因此想要体验 bullseye 最好的办法是新安装一个纯净的 debian10 然后通过软件升级直接升
到 bullseye

下载 buster live cd，推荐清华大学开源镜像站 [debian-cd](https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current-live/)

# 安装常用的软件

| 软件名称   | 备注                                  |
| virtualbox | 安装ubuntu20.04的版本，可正常完美使用 |
|            |                                       |

## 静态博客 jekyll

### ruby

ruby，常规安装即可

```
$ sudo apt install ruby-dev ruby
```

更换国内 gem 镜像

```
$ gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
$ gem sources -l
https://gems.ruby-china.com
# 确保只有 gems.ruby-china.com
```

# 升级后的优化

## 安装常用的插件

1. gnome-shell-extension-dashtodock
2. gnome-shell-extension-draw-on-your-screen
3. gnome-shell-extension-easyscreencast
4. gnome-shell-extension-gpaste
5. gnome-shell-extension-move-clock
6. gnome-shell-extension-no-annoyance
7. gnome-shell-extension-remove-dropdown-arrows
8. gnome-shell-extension-top-icons-plus
9. gnome-shell-extension-trash
10. gnome-shell-extensions
11. no-title-bar 这个原始作者已不再维护，在新 gnome 下已经不可使用，但已有其他人 fork 了该插件，debian11 可以正常使用
https://extensions.gnome.org/extension/2015/no-title-bar-forked/

## 安装numix主题
```
sudo apt install numix-gtk-theme numix-icon-theme
```
## dash to dock 关闭快捷按键
平常使用 `<Super>+1..9` 用来快速切换 emacs 的 buffer 以及终端的 tab，已经习惯了，虽然默认用来启动 dock 的应用程
序虽然很方便，但是不太符合我自己的习惯。根据以往的经验，本来之 `dash-to-dock` 插件可以直接关闭掉，但是发现失效了
最终找到这个问题记录 [issue](https://gitlab.gnome.org/GNOME/gnome-shell/-/issues/1250)，通过如下关闭该快捷键盘

```bash
for i in {1..9}; do 
  gsettings set "org.gnome.shell.keybindings" "switch-to-application-$i" "[]" 
  gsettings set "org.gnome.shell.extensions.dash-to-dock" "app-hotkey-$i" "[]" 
done
```

执行后可以通过下面的命令，查看

```
gsettings list-recursively | grep -E "switch-to-(workspace|application)-[0-9]" | sort -n
org.gnome.desktop.wm.keybindings switch-to-workspace-10 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-11 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-12 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-1 ['<Super>Home']
org.gnome.desktop.wm.keybindings switch-to-workspace-2 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-3 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-4 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-5 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-6 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-7 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-8 @as []
org.gnome.desktop.wm.keybindings switch-to-workspace-9 @as []
org.gnome.shell.keybindings switch-to-application-1 @as []
org.gnome.shell.keybindings switch-to-application-2 @as []
org.gnome.shell.keybindings switch-to-application-3 @as []
org.gnome.shell.keybindings switch-to-application-4 @as []
org.gnome.shell.keybindings switch-to-application-5 @as []
org.gnome.shell.keybindings switch-to-application-6 @as []
org.gnome.shell.keybindings switch-to-application-7 @as []
org.gnome.shell.keybindings switch-to-application-8 @as []
org.gnome.shell.keybindings switch-to-application-9 @as []
```

## 中州韵输入法安装和美化-ibus
参见 [Rime输入法配置](/blog/rime-tweak)
