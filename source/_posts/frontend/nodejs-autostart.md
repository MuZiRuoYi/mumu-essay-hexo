---
title: CentOS nodejs 自启动
date: 2019-04-28 22:27:53
tags: [Frontend, nodejs]
category: 'code'
---

使用 nodejs 开发一个 Web 后台，在部署到服务器前，在本地虚拟机搭建一套环境模拟部署，同时用于发布前预览。由于是虚拟机，会叫多次重启虚拟机，每次都需要重启 nodejs 服务就显得很繁琐，于是就查资料做了一个自启动脚本，此处记录最终调研后的可用结果。

### 新建 shell 脚本

对于 CentOS 6.9（本人虚拟机都是使用该版本）来说，一般开机自启动为`chkconfig <APP name> on`来设置，但是对于 nodejs 开发的 APP 来说，更过情况是进入目录，通过`npm`启动，并没有现成的什么命令可以用来自启，于是就想到写一个`shell`脚本来实现自启动。

    $ vim autostart.sh

文件内容为：

```sh
#!/bin/sh
#chkconfig: 2345 80 90
#description: 此处是解释说明
cd /data/app/server
npm run stop
npm run start
```

注释：

- 第一行：告诉系统使用的 shell
- 第二行：表示在 2/3/4/5 运行级别启动，启动序号(S80)，关闭序号(K90)
- 第三行：解释说明

### 移动脚本

将脚本移动到/etc/rc.d/init.d/目录下：

    $ mv ./autostart.sh /etc/rc.d/init.d/

### 赋权

默认新建脚本没有可执行权限（具体可以使用`ll`或`ls -al`命令进行查看权限），所以需要添加可执行权限：

    $ chmod +x /etc/rc.d/init.d/autostart.sh

### 设置脚本开机自启动

此处使用到`chkconfig`命令来设置`shell`脚本自启动：

    $ chkconfig --add autostart.sh
    $ chkconfig autostart.sh on
