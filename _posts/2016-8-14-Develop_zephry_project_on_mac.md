---
layout:     post
title:      "zephyr项目在macos平台的开发"
subtitle:   "macOS平台环境设置"
date:       2016-8-14 12:19:30
header-img: "img/post-bg-zephyr.png"
author:     darcylee
catalog:    true
tags:       [学习笔记, zephyr]

---

## 关于`zephyr™`项目

[zephyr™](https://www.zephyrproject.org), 首先它是一个开源的项目，主要目的是针对资源有限的系统设计的实时的系统内核，可以胜任从简单的嵌入式传感器到复杂的物联网网关等应用场景。官方的文档也相对比较完整，可以[移步官网了解一下](https://www.zephyrproject.org/doc/)。
该项目开源于今年2月份，到目前版本已升级到1.5.0

获取源代码

```
$ git clone https://gerrit.zephyrproject.org/r/zephyr
```
zephyr™使用类似linux内核`Kbuild`构建系统来组织整个项目的，相信有一定嵌入式linux开发经验的童鞋们，看了官网的[QuickStart](https://www.zephyrproject.org/doc/getting_started/getting_started.html)文档后，很容易上手开发的， 文档中提供了三个不同平台`linux`、`windows`、`macOS`的开发环境设置介绍，如果你目前使用的是`linux`或者`windows`平台可以移步[官网]()，寻找相对应平台的说明。这里我们要说的是基于`macOS`平台的开发环境设置。

虽然官网有给出`macOS`平台的[开发环境设置指南](https://www.zephyrproject.org/doc/getting_started/installation_mac.html)，但是还是会碰到一些问题，我这里记录了一下我的设置过程，以及我碰到的问题的解决办法，希望能帮到和我一样想在`macOS`平台上做基于`zephyr`的嵌入式开发的朋友。

## 开发环境设置

* 首先请把系统升级到`Mac OS X 10.11 (El Capitan)`，我是基于这个版本来设置的，官网也有相应的声明。

#### 安装依赖包

安装`Homebrew`， 安装过的同学跳过， `Homebrew`好比debian系Linux系统里的`apt-get`工具，很好很强大。安装很简单，移步[Homebrew官网](http://brew.sh)，按照上面的说明安装。

继续安装一些工具

```
$ brew install gettext qemu help2man mpfr gmp coreutils wget python3

$ brew tap homebrew/dupes

$ brew install grep --with-default-names

$ pip3 install ply

$ brew install gettext
```

稍作设置

```
$ brew edit crosstool-ng
```
添加下面一行到`depends`section

	depends_on 'grep' => '--default-names'

安装`Crosstool-ng`

```
$ brew install crosstool-ng

$ brew link --force gettext
```

#### 编译工具链

`macOS`使用的文件系统对文件名称大小写是不敏感的，而编译工具链需要一个大小写敏感的文件系统，所以我们要为编译工作提供一个大小写敏感的分区作为工具链编译空间。使用`macOS`自带的`diskutil`可以很方便的创建一个出来。创建一个8G左右的分区,分区名称可以任意，我这里叫`excs`，**_注意Format的选择_**
![img](/img/in-post/post_zephyr_on_mac/creat_case-sensitive.png)

创建好后，在`／Volumes`下面会多出一个文件夹，也就是你创建的新分区。

```
$ ls /Volumes/
Macintosh HD excs
```
进入`excs`（根据你创建的分区名称）

```
$ cd /Volumes/excs

$ mkdir CrossToolNG

$ cd CrossToolNG

$ mkdir build src x-tools

$ cd build
```

复制zephyr项目下面的配置文件到`build`目录，zephyr提供了2个平台的配置x86、arm，我这里选择的是arm，因为我的目标平台是嵌入式arm平台。`${ZEPHYR_BASE}`为你zephyr项目的根目录

```
$ cp ${ZEPHYR_BASE}/scripts/cross_compiler/arm.config .config
```
有配置过`Crosstool-ng`的童鞋也可以自行配置，这里不再说。

开始构建工具链，首先

```
$ ct-ng menuconfig
```
执行上面的命令，进行简单的配置后退出保存，主要配置编译输出的一些目录，下图是我的配置
![img](/img/in-post/post_zephyr_on_mac/ct-ng_config1.png)


打开`.config`文件再次确认下路径设置

```
...
#
# Paths
#
CT_LOCAL_TARBALLS_DIR="/Volumes/excs/CrossToolNG/src"
CT_SAVE_TARBALLS=y
CT_WORK_DIR="${CT_TOP_DIR}/.build"
CT_PREFIX_DIR="/Volumes/excs/CrossToolNG/x-tools/${CT_TARGET}"
CT_INSTALL_DIR="${CT_PREFIX_DIR}"
...
＃ 下面2行很重要，必须为n，不然编译不过
CT_WANTS_STATIC_LINK=n
CT_CC_STATIC_LIBSTDCXX=n
...
```

在开始编译之前，我推荐使用`Homebrew`安装gcc，不要使用`macOS`自带的gcc版本，我一开始使用自带的gcc版本编译错误，当时没有截图， 错误信息没办法提供给你们看了。安装gcc命令很简单， 如下：

```
$ brew install gcc
```
过程很漫长，差不多20分钟左右（15年中旬15寸低配macbookpro）， 在安装过程中本本风扇狂转。。。我也是醉了。。回归正题

安装完成后确认一下

```
$ gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/local/Cellar/gcc/5.3.0/libexec/gcc/x86_64-apple-darwin15.6.0/5.3.0/lto-wrapper
Target: x86_64-apple-darwin15.6.0
Configured with: ../configure --build=x86_64-apple-darwin15.6.0 --prefix=/usr/local/Cellar/gcc/5.3.0 --libdir=/usr/local/Cellar/gcc/5.3.0/lib/gcc/5 --enable-languages=c,c++,objc,obj-c++,fortran --program-suffix=-5 --with-gmp=/usr/local/opt/gmp --with-mpfr=/usr/local/opt/mpfr --with-mpc=/usr/local/opt/libmpc --with-isl=/usr/local/opt/isl --with-system-zlib --enable-libstdcxx-time=yes --enable-stage1-checking --enable-checking=release --enable-lto --with-build-config=bootstrap-debug --disable-werror --with-pkgversion='Homebrew gcc 5.3.0' --with-bugurl=https://github.com/Homebrew/homebrew/issues --enable-plugin --disable-nls --enable-multilib --with-native-system-header-dir=/usr/include --with-sysroot=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk
Thread model: posix
gcc version 5.3.0 (Homebrew gcc 5.3.0)
```

一切准备就绪，ok，开始编译工具链

```
$ ct-ng build
```

又是一个漫长的等待，大概15分钟左右，提示编译成功。我们察看一下`x-tools`文件夹

```
$ cd /Volumes/excs/CrossToolNG/x-tools

$ ls
arm-none-eabi
```
交叉编译工具链产生了，试验一下

```
$ cd arm-none-eabi/bin

$ ./arm-none-eabi-gcc -v
Using built-in specs.
COLLECT_GCC=./arm-none-eabi-gcc
COLLECT_LTO_WRAPPER=/Volumes/excs/CrossToolNG/x-tools/arm-none-eabi/libexec/gcc/arm-none-eabi/5.2.0/lto-wrapper
Target: arm-none-eabi
Configured with: /Volumes/excs/CrossToolNG/build/.build/src/gcc-5.2.0/configure --build=x86_64-build_apple-darwin15.6.0 --host=x86_64-build_apple-darwin15.6.0 --target=arm-none-eabi --prefix=/Volumes/excs/CrossToolNG/x-tools/arm-none-eabi --with-local-prefix=/Volumes/excs/CrossToolNG/x-tools/arm-none-eabi/arm-none-eabi/sysroot --with-sysroot=/Volumes/excs/CrossToolNG/x-tools/arm-none-eabi/arm-none-eabi/sysroot --with-newlib --enable-threads=no --disable-shared --with-pkgversion='crosstool-NG crosstool-ng-1.22.0' --with-float=soft --enable-__cxa_atexit --with-gmp=/Volumes/excs/CrossToolNG/build/.build/arm-none-eabi/buildtools --with-mpfr=/Volumes/excs/CrossToolNG/build/.build/arm-none-eabi/buildtools --with-mpc=/Volumes/excs/CrossToolNG/build/.build/arm-none-eabi/buildtools --with-isl=/Volumes/excs/CrossToolNG/build/.build/arm-none-eabi/buildtools --with-cloog=/Volumes/excs/CrossToolNG/build/.build/arm-none-eabi/buildtools --with-libelf=/Volumes/excs/CrossToolNG/build/.build/arm-none-eabi/buildtools --enable-lto --enable-target-optspace --disable-libgomp --disable-libmudflap --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-nls --disable-multilib --enable-languages=c
Thread model: single
gcc version 5.2.0 (crosstool-NG crosstool-ng-1.22.0)
```

没有问题，很好

回到`zephyr`工程目录

```
$ cd ${ZEPHYR_BASE}

$ source zephry_env.sh
```

编译一个例子程序helloworld试试， go

```
$ cd samples/hello_world/microkernel
```

在编译前，先设置2个环境变量

```
$ export ZEPHYR_GCC_VARIANT=xtools

$ export XTOOLS_TOOLCHAIN_PATH=/Volumes/excs/CrossToolNG/x-tools
```

编译helloworld

```
$ make BOARD=frdm_k64f
Using /Users/dl/work/proj/ex_projects/zephyr/boards/frdm_k64f/frdm_k64f_defconfig as base
Merging /Users/dl/work/proj/ex_projects/zephyr/kernel/configs/nano.config
Merging prj.conf
#
# configuration written to .config
#
  GEN     ./Makefile
scripts/kconfig/conf --silentoldconfig Kconfig
  Using /Users/dl/work/proj/ex_projects/zephyr as source for kernel
  GEN     ./Makefile
  CHK     include/generated/version.h
  UPD     include/generated/version.h
  CHK     misc/generated/configs.c
.
.
.
  LD      src/built-in.o
  AR      libzephyr.a
  LINK    zephyr.lnk
  BIN     zephyr.bin
```

没有错误，编译成功，最终生成的`bin`文件在outdir文件下

察看编译出来的`elf`文件，确实是arm平台的

```
$ file zephyr.elf
zephyr.elf: ELF 32-bit LSB executable, ARM, version 1 (SYSV), statically linked, not stripped
```

到这里你的环境基本上配置好了。

可以把上面的环境变量设置到`~/.zephyrrc`中，这样下次就不需要重新export了

```
$ cat ~/.zephyrrc
export XTOOLS_TOOLCHAIN_PATH=/Volumes/CrossToolNG/x-tools
export ZEPHYR_GCC_VARIANT=xtools
```

如果你觉得最开始分割到那8G左右到空间比较浪费，我们可以做一个`dmg`，这样有用的时候打开这个`dmg`就可以了。

打开`diskutil`工具，选择`File->New Image->Image from folder`,选择`/Volumes/excs/CrossToolNG/x-tools`文件夹
![img](/img/in-post/post_zephyr_on_mac/make_xtool_dmg.png)

生成一个`dmg`![img](/img/in-post/post_zephyr_on_mac/xtool_dmg.png)，有用的时候打开, 就会在`／Volumes`，多出`x-tools`文件夹

```
$ ls ／Volumes
Macintosh HD   x-tools
```
然后环境变量做相应的变更

```
export XTOOLS_TOOLCHAIN_PATH=/Volumes/x-tools
```
这样，就可以把8G的分区`excs`删除，把磁盘空间还给`macOS`系统了。



## 使用`menuconfig`配置工程遇到的错误

具体错误如下

```
$ cd ${ZEPHYR_BASE}/samples/hello_world/microkernel

$ make menuconfig
  GEN     ./Makefile
  HOSTLD  scripts/kconfig/mconf
Undefined symbols for architecture x86_64:
  "_acs_map", referenced from:
      _print_arrows in checklist.o
      _dialog_checklist in checklist.o
      _dialog_clear in util.o
      _draw_box in util.o
      _dialog_inputbox in inputbox.o
      _dialog_textbox in textbox.o
.
.
.

```

解决办法

```
$ brew install ncurses
```

然后在`~/.zephyrrc`中添加`ncurse`pkgconfig

```
export PKG_CONFIG_PATH=/usr/local/opt/ncurses/lib/pkgconfig:$PKG_CONFIG_PATH
```
