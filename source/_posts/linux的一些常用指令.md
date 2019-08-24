---
title: linux的一些常用指令
date: 2019-07-30 02:22:16
tags:
 - linux
 - 常用
categories: 实习
---
一些使用频率较高的`linux`指令
<!--more-->
### 在哪&&到哪去

*   显示当前路径，`pwd`
*   显示当前路径下文件，`ls -l -a(可以显示隐藏的文件)`
*   返回上一级目录，`cd ..`
*   返回根目录，`cd ~`
*   进入文件夹，`cd [file_name]/[file_name]/[file_name]`

### 删除&&创建&&拷贝&&移动

*   删除文件，`rm -f [file_name]`
*   删除文件夹，`rm -rf [file_name]`
*   创建文件，`touch [file_name]`
*   创建文件夹，`mkdir [file_name]`
*   拷贝文件，`cp [file_from] [file_to]`
*   移动文件&&重命名文件，`mv [file_from] [file_to]`

### 一些工具指令

*   利用apt-get下载工具，`apt-get install [file_name]`
*   利用vim查看文件，`vim [file_name]`

### 一些提高效率的指令（oh-my-zsh+zsh）

*   自动补齐已存在文件名，`Tab`
*   获得root权限，`sudo su`
*   列出当前会话已经访问过的目录，`d`
*   历史记录，`在工具命令后，按上下键可以查看历史记录`
*   通配符，`*(可以替代目录中的字段，可以简化删除等操作)`
*   git的简写指令，`google一下`
