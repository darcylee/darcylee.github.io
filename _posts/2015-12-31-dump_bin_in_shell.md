---
layout:     post
title:      "两个命令行察看二进制文件的工具"
subtitle:   "hexdump、xxd"
date:       2015-12-31 10:19:30 +0800
header-img: "img/post-bg-debian.jpg"
author:     darcylee
tags:       Linux shell
---

> 1. hexdump
> 2. xxd

## hexdump
hexdump命令一般用来查看“二进制”文件的十六进制编码，但实际上它能查看任何文件，而不只限于二进制文件

命令行输入：`hexdump --help`查看详细介绍：


```
lijq@server » hexdump --help
hexdump: invalid option -- '-'
usage: hexdump [-bcCdovx] [-e fmt] [-f fmt_file] [-n length]
               [-s skip] [file ...]
       hd      [-bcdovx]  [-e fmt] [-f fmt_file] [-n length]
               [-s skip] [file ...]
               
```

语法选项

> huxdump [选项] [文件] ...
> 
> -n length 只格式化输入文件的前length个字节。 
> -C 输出规范的十六进制和ASCII码。 
> -b 单字节八进制显示。 
> -c 单字节字符显示。 
> -d 双字节十进制显示。 
> -o 双字节八进制显示。 
> -x 双字节十六进制显示。 
> -s 从偏移量开始输出。 
> -e 指定格式字符串，格式字符串包含在一对单引号中，格式字符串形如：'a/b "format1" "format2"'。

每个格式字符串由三部分组成，每个由空格分隔，第一个形如a/b，b表示对每b个输入字节应用format1格式，a表示对每a个输入字节应用format2格式，一般a>b，且b只能为1，2，4，另外a可以省略，省略则a=1。format1和format2中可以使用类似printf的格式字符串，如：

> %02d：两位十进制 
> %03x：三位十六进制 
> %02o：两位八进制 
> %c：单个字符等

还有一些特殊的用法：

> %\_ad：标记下一个输出字节的序号，用十进制表示。 
> %\_ax：标记下一个输出字节的序号，用十六进制表示。 
> %\_ao：标记下一个输出字节的序号，用八进制表示。 
> %\_p：对不能以常规字符显示的用 . 代替。

同一行如果要显示多个格式字符串，则可以跟多个-e选项。

示例：

```
lijq@server » hexdump -e '16/1 "%02X " " | "' -e '16/1 "%_p" "\n"' rootfs.ubi
55 42 49 23 01 00 00 00 00 00 00 00 00 00 00 00 | UBI#............
00 00 08 00 00 00 10 00 2A AD C9 72 00 00 00 00 | ........*..r....
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 | ................
00 00 00 00 00 00 00 00 00 00 00 00 F2 06 B0 A2 | ................
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF | ................
*
55 42 49 21 01 01 00 05 7F FF EF FF 00 00 00 00 | UBI!............
...
...
```


部分引用自[linuxde](http://man.linuxde.net/hexdump)

## xxd

`xxd --help` 查看详细使用方法

```
lijq@server » xxd --help
Usage:
       xxd [options] [infile [outfile]]
    or
       xxd -r [-s [-]offset] [-c cols] [-ps] [infile [outfile]]
Options:
    -a          toggle autoskip: A single '*' replaces nul-lines. Default off.
    -b          binary digit dump (incompatible with -ps,-i,-r). Default hex.
    -c cols     format <cols> octets per line. Default 16 (-i: 12, -ps: 30).
    -E          show characters in EBCDIC. Default ASCII.
    -g          number of octets per group in normal output. Default 2.
    -h          print this summary.
    -i          output in C include file style.
    -l len      stop after <len> octets.
    -ps         output in postscript plain hexdump style.
    -r          reverse operation: convert (or patch) hexdump into binary.
    -r -s off   revert with <off> added to file positions found in hexdump.
    -s [+][-]seek  start at <seek> bytes abs. (or +: rel.) infile offset.
    -u          use upper case hex letters.
    -v          show version: "xxd V1.10 27oct98 by Juergen Weigert".
```

语法

> xxd [options] [infile [outfile]]
> xxd -r [-s [-]offset] [-c cols] [-ps] [infile [outfile]]

常用选项

> -b: 以二进制格式显示，每个字符将被转化成8个0/1的数据
> -l: 只显示N个字符，`-l N`
> -s: [+][-]seek 从infile的绝对或者想对位置开始显示， `-s offset`
> -u: 以大写字母输出，默认小写字母输出
> -g: 显示方式，`-g N` 以N个byte为一组输出，默认是2

例子

>从0x500000处开始显示64个字节

```
lijq@server » xxd -s 0x500000 -l 64 rootfs.ubi
0500000: 5542 4923 0100 0000 0000 0000 0000 0000  UBI#............
0500010: 0000 0800 0000 1000 2aad c972 0000 0000  ........*..r....
0500020: 0000 0000 0000 0000 0000 0000 0000 0000  ................
0500030: 0000 0000 0000 0000 0000 0000 f206 b0a2  ................
```
这个对调试通讯协议出错的时候特别有用
