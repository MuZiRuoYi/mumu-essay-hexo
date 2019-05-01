---
title: Apache 安装与配置
date: 2019-04-27 20:33:40
tags: [Linux, CentOS]
category: 'linux'
---

Linux 服务器最常用的 Web 服务 APP 为 Apache 和 nginx，此处讲述 Apache（在部署 WordPress 时候遇到，开始首次正式使用 Apache，所以文中牵扯到都是本人部署所需要的配置记录）。

### 安装

运行命令：

    $ yum install httpd

启动/停止/重启：

    $ service httpd start
    $ service httpd stop
    $ service httpd restart

设置开机重启：

    $ chkconfig httpd on

### 启用 PHP

编辑配置文件：

    $ vim /etc/httpd/conf/httpd.conf

将

```apache
AllowOverride None
DirectoryIndex index.html index.html.var
```

改为：

```apache
AllowOverride All
DirectoryIndex index.php index.html index.html.var
```

在

```apache
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz
```

后添加：

```apache
AddType application/x-httpd-php .php
```

### 配置其他端口

编辑配置文件：

    $ vim /etc/httpd/conf/httpd.conf

在文件最后添加：

```apache
Include /data/app/config/*.httpd.conf
```

其中路径是你想要添加的配置文件路径与文件名，该配置文件，比如开启 82 端口，如下配置所示：

```apache
Listen 82
NameVirtualHost *:82

<VirtualHost *:82>
  ServerName localhost
  DocumentRoot "/data/app/www"
  <IfModule dir_module>
    DirectoryIndex index.php
  </IfModule>
</VirtualHost>
```

### 斜杠问题

默认情况下，网页目录的最后必须加入斜杠`/`，但是平时开发很少会主动添加上这一个斜杠，例如：

可以浏览`http://www.example.com/abc/`，但是不能浏览`http://www.example.com/abc`，就是说浏览目录时最后必须加`/`。当碰到`abc`时候，会默认查找`abc`文件，而不是当做目录去查找。

#### 解决

在`httpd.conf`里，将

    UseCanonicalName On

把`On`修改为`Off`就可:

    UseCanonicalName Off
