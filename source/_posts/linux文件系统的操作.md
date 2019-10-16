---
title: linux文件系统的操作
date: 2019-10-15 23:07:41
tags:
- Linux
---

## linux磁盘和文件系统的操作

linux磁盘和文件系统的操作主要涉及到创建分区， 格式化分区（创建文件系统）和挂载这三步操作，本文主要介绍这三步操作用到的工具和文件系统的一些相关内容。

### 磁盘的分区

分区工具有：
- parted：一个文本命令工具，支持MBR和GPT。
- gparted： parted的图形版本
- fdisk： Linux传统的文本命令分区工具，不支持GPT
- gdisk： fdisk的另一个版本，支持GPT，但不支持MBR

查看分区表命令：

`$ parted -l`

重新加载磁盘上的分区表：

`$ blockdev --rereadpt /dev/sdf`

### 文件系统

Linux支持的文件系统有第四扩展文件系统（ext4）、ISO 9660（ iso9660）、 FAT文件系统（ msdos、 vfat、 umsdos）和HFS+（ hfsplus）等，下面讲解文件系统的一些操作：

#### 创建文件系统

创建文件系统的工具是mkfs，mkfs是一系列文件系统创建程序的前端界面，如mkfs.fs。 fs是一种文件系统类型。当运行mkfs -t ext4时，实际上运行的是mkfs.ext4。ru

如在/dev/sdf2上创建ext4分区：

`$ mkfs -t ext4 /dev/sdf2`

查看命令的软连接：

`$ ls -l /sbin/mkfs.*`

#### 挂载文件系统

要挂载文件系统，需要有如下数据：

- 文件系统所在设备（如磁盘分区，文件系统数据存放的位置）
- 文件系统类型
- 挂载点

mount命令可以查看当前文件系统状态，至于文件系统的卸载，可以使用umount命令

例如要挂载/dev/sdf2设备到/home/extra，可以运行下面的命令：

`mount -t ext4 /dev/sdf2 /home/extra`

一般更好的做法是使用文件系统的UUID来挂载，而不是文件设备名，可以使用blkid（ block ID）命令查看设备和其对应的文件系统及UUID，然后使用 UUID=来通过UUID挂载文件系统，如：

`mount UUID=a9011c2b-1c03-4288-b3fe-8ba961ab0898 /home/extra`