---
title: Arch Linux - 常用快捷键（可自己设置）及其命令
date: 2018-03-29 23:43:08
tags: [archLinux, cmd]
---

### 系统配置

ArchLinux/i3

### 常用快捷键

#### 系统

    $: $mod + shift + r // 重启i3
    $: $mod + shift + e // 关闭i3

#### App 启动

    $: $mod + d // 打开搜索
    $: $mod + z // 打开菜单
    $: $mod + F2 // 启动chrome（自定义设置）
    $: $mod + F3 // 打开文件管理（自定义设置）
    $: $mod + shift + h // 打开帮助文档

#### 窗口布局

    $: $mod + number // 切换/打开工作区
    $: $mod + shift + number // 把当前窗口移动到某个工作区
    $: $mod + shift + direction // 当前窗口移动到某个标签
    $: $mod + direction // 切换窗口
    $: $mod + shift + q // 关闭窗口
    $: $mod + w // 标签形式显示（类似于浏览器）
    $: $mod + e // 切换水平/垂直均分
    $: $mod + s // 层叠
    $: $mod + shift + space // 切换浮动
    $: $mod + h // 横向排列窗口
    $: $mod + v // 纵向排列窗口
    $: $mod + f // 窗口全屏

### 常用命令

#### 代理

    $: proxychains npm install ****

#### 重启 ssl

    $: sudo systemctl restart sslocal.service

#### WPS

    $: wps // Word
    $: et // Excel
    $: wpp // PPT

#### VMware

    $: sudo /etc/init.d/vmware start // 重启电脑后VMware报错

#### 磁盘

    $: sudo journalctl --disk-usage  // 日志使用磁盘大小
    $: sudo journalctl --vacuum-time=1months  // 设置保留一个月内日志
    $: sudo du -t 100M / -h  // 查看磁盘占用情况
