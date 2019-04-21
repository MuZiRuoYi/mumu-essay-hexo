---
title: 部署 Hexo Pages 到自己的服务器
date: 2018-06-03 14:09:52
tags: [Hexo]
category: 'hexo'
---

本来构建一个 Hexo pages 部署到 GitHub 挺好的，但是总有一些例外。

本来我想搭建一个 wiki，记录一下公司前端团队的一些新人入职须知，项目组件 API，外加一些团队开发总结什么的，我找了一些 wiki，但是界面设计被我嫌弃了，然后我想起了自己的 GitHub Pages，如果吧 build 后内容使用 nginx 服务跑起来，会不会很完美？

然后就试了一把，发现真的很不错，于是就新建了一个项目，前端团队一起维护。可是接着问题就来了，部署怎么办呢？GitHub Pages 支持`hexo deploy`直接部署，我自己的服务器可不支持。

查找了一些资料，发现了`git hooks`，那么那么怎么做呢？

下文中本人使用的服务器系统是 CentOs 6.5。

### 安装 App

安装 nginx、git（具体安装不再详述）：

    $ yum install nginx
    $ yum install git

### 初始化 Git 仓库

    $ sudo mkdir /data/www/wiki
    $ cd /data/www/wiki
    $ sudo mkdir app // 存放 wiki 源码
    $ sudo git init --bare wiki.git

使用`--bare`参数，Git 就会创建一个裸仓库，裸仓库没有工作区，我们不会在裸仓库上进行操作，它只为共享而存在。

### 配置 git hooks

    $ vim /wiki.git/hooks/post-receive

在 post-receive 文件中写入如下内容:

```config
GIT_REPO=/data/www/wiki/wiki.git
TMP_GIT_CLONE=/data/www/wiki/app_temp
PUBLIC_HEXO=/data/www/wiki/app
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_HEXO}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_HEXO}
```

设置文件夹可执行权限：

    $ chmod +x post-receive

### 添加 git 帐号

为了安全考虑，为服务器创建 git 帐号，禁止 shell 登录，但是能够正常使用 git。

添加帐号：

    $ sudo adduser git

设置密码：

    $ passwd git

改变权限：

    $ vim /etc/passwd

将

```hljs
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```hljs
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

git 帐号拥有`/data/www/wiki/`目录权限

    sudo chown -R git:git /data/www/wiki

### 配置源码\_config.yml

```yml
deploy:
  type: git
  repo: git@<服务器IP地址/服务器域名>:/data/www/wiki/wiki.git
  branch: master
```

接下来，就可以使用：

    $ hexo clean && hexo g && hexo d

进行部署了，去服务器发现`/data/www/wiki/app`里已经是 build 后代码，那么接下来该使用 nginx 把站点跑起来了。

### nginx 搭建站点

不通方式安装的 nginx 可能搭建站点不同，我是使用的 yum 安装的 nginx。

    $ sudo vim /etc/nginx/conf.d/wiki.conf

添加以下内容（怎么配置 nginx 站点，此处不多记录）：

```conf
server {
    # 开启gzip
    gzip on;
    gzip_min_length 1k;
    gzip_comp_level 2;
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png font/ttf font/otf image/svg+xml;
    gzip_vary on;
    gzip_disable "MSIE [1-6]\.";

    listen       8080;
    listen       [::]:8080;
    server_name  wiki.fronted.com;
    root         /data/www/wiki/app;

    # Load configuration files for the default server block.
    # include /etc/nginx/default.d/*.conf;

    index index.html index.htm;

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swfi|woff)$ {
        expires 30d;
    }

    location ~ .*\.(js|css)$ {
        expires 1h;
    }
}
```

然后重启 nginx：

    $ service nginx restart

之后就可以去浏览器访问了。

### 免密部署（未完成）

配置好后发现每次部署都需要输入密码，个人比较烦，所以就寻找一些免密部署方案，虽然尝试了一些，但是到写这篇文章的时候还没有尝试出来，等完成后后续修改该文章。

如果您看到这篇文章，欢迎评论。

### 注意

本人发现，每次 deploy 的时候，如果没有任何更改，将不重新部署。即使你去服务器上把文件删除了，没有修改的情况话运行`hexo deploy`依然不会进行再次部署。
