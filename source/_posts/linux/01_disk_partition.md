---
title: Linux 学习笔记_01 - 磁盘分区
date: 2018-06-12 11:59:20
tags: [Linux]
category: 'linux'
---

磁盘由盘片、机械手臂、磁头、主轴马达组成，数据实际上是写入盘片。

盘片上可以细分为**扇区（Sector）**和**柱面（Cylinder）**两种单位，每个扇区 512bytes。

磁盘的第一个扇区记录了：

1.  **主引导分区（Master Boot Record， MBR）**：可以安装引导加载程序的地方，共 446bytes
2.  **分区表（partition table）**：记录整个磁盘分区状态，共 64bytes
