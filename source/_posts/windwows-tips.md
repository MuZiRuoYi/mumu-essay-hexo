---
title: Windows 小问题总结
date: 2018-03-30 13:12:49
tags: cmd
---

### 命令行

#### DNS

    $: ipconfig /flushdns  // 刷新DNS

#### nginx（在 nginx 安装目录运行 CMD）

    $: nginx -s reload  // 重新加载 nginx 配置（修改配置后运行）
    $: nginx -s stop  // 停止 nginx，不保存相关信息
    $: nginx -s quit  // 停止 nginx，保存相关信息
    $: nginx -h  // 查看帮助，可以使用其他相关命令

### Windows 10 权限问题

修改注册表 regedit:

    HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System

修改这个路径下的键值：`EnableLUA`从`1`设置为`0`

### hosts 文件目录

    C:/Windows/System32/drivers/etc/

### 添加开机启动项

将快捷方式放到文件夹：`C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`

### Sass 支持错误

gyp 是一种根据 c++源代码编译的工具，node-gyp 就是为 node 编译 c++扩展的时候使用的编译工具。

> Module build failed (from ./node_modules/\_sass-loader@7.1.0@sass-loader/lib/loader.js):\
> Error: Node Sass does not yet support your current environment: Windows 64-bit with Unsupported runtime (67)

解决方案：

    $: npm install -g node-gyp
    $: npm install -g windows-build-tools  // 为 node-gyp 配置安装 python2.7 以及 VC++ build Tools 依赖
    $: npm rebuild node-sass
