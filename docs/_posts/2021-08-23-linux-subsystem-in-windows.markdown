---
layout: single
toc: true
toc_label: "Table of contents"
title:  "Linux subsystem in Windows"
date:   2021-08-23 17:16:00 +0800
categories: Windows Linux OperatingSystem
---

Redis没有Windows版本，所以需要使用Linux。Windows提供了Linux子系统，而不必安装虚拟机，比起我在本科时方便多了。虽然早就知道了这个新特性，但是一直没机会用，因为不怎么使用Linux开发环境，这次正好配置一下。

1. 首先需要在控制面板 -> 程序 -> 启用或关闭Windows功能 中启用适用于Linux的Windows子系统，然后会自动安装。安装完成后会需要重启电脑。
2. 重启后在Windows商店下载安装Linux系统，搜索Linux后安装Ubuntu，其他基于Linux的操作系统也行。
3. 在Windows商店安装完成后启动Ubuntu，还会花费几分钟完成安装，基本就是两三分钟。
4. 完成安装后需要新建用户和密码。
5. 然后就进入命令行了，可以开始使用Linux了。


# 参考资源

[Windows Subsystem for Linux (WSL) Tutorial & How To [Youtube]](https://www.youtube.com/watch?v=av0UQy6g2FA)
>安装Linux子系统的视频教程。