---
title: 展现结构化数据流的shell--NuShell
date: 2019-11-05 16:50:41
tags:
- 笔记
---

### 展现结构化数据流的shell--NuShell

最近发现了一个新的开源shell--NuShell/，尝试了一下后，顿时眼前一亮，命令输出的数据都是以表格形式展示的。它的特点就像官方文档描述的它的设计哲学，不同于Windows的powershell和linux的bash把文件和服务看做是文本流，它把每一个输出的数据都看成是一个结构化的数据，所以我们看到它命令输出的数据会以表格的形式来展示。当然NuShell也支持非结构化的数据，NuShell是用rust语言开发的，所以安装之前先要安装rust，具体安装和试用方式可以查看{% link github项目地址 https://github.com/nushell/nushell %} {% link gitbook文档 https://book.nushell.sh/ %}