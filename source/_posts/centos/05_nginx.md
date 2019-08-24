---
title: nginx 安装与配置
date: 2019-05-19 10:35:29
tags: [Linux, CentOS, nginx]
category: 'linux'
---

nginx 是一款我比较常用的应用，我个人很多情况下都会使用它来作为唯一对外的 Web 应用，其他各种 Web 服务通过 nginx 来做代理。尤其作为一名前端开发，将构建好的静态页面放到一个文件夹，将 nodejs 作为服务端，通过 nginx 做反向代理来对外，因此 nodejs 仅需要保证端口在服务器内或内网可通信即可。同时，开发后作为演示环境也配置比较方便。（个人使用都是基于 CentOS6.9）

### 安装

安装很方便：

    $: yum install nginx

如果直接安装运行以上失败，可以先添加源（CentOS7 安装 nginx 经常进行这个操作）：

    $: rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
    $: yum install nginx

启动/停止/重启：

    $ service nginx start
    $ service nginx stop
    $ service nginx restart

设置开机启动：

    $ chkconfig nginx on

重新加载配置：

    $ cd <config pwd>
    $ nginx -s reload

### 配置

查找 nginx 配置文件位置：

    $ nginx -t

对于 CentOS6.9，输出如下：

    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful

#### 多位置配置文件

    $ vi /etc/nginx/nginx.conf

添加`include <conf path>;`，如下：

```conf
http {
  ...
  include /data/app/config/*.nginx.conf;
}
```

**注意**：nginx 配置每行尾需要分号“;”。

#### `location`

    location [=|~|~*|^~] /uri/ { ··· }

`=?`：开头表示精确匹配

`^~?`：开头表示 uri 以某个常规字符串开头，理解为匹配 url 路径即可。nginx 不对 url 做编码，因此请求为`/static/20%/aa`，可以被规则`^~ /static/ /aa` 匹配到。

`~?`：开头表示区分大小写的正则匹配

`~*?`：开头表示不区分大小写的正则匹配

`!~`：区分大小写不匹配

`!~*`：分别为及不区分大小写不匹配?的正则

`/`：通用匹配，任何请求都会匹配到

多个`location`配置的情况下匹配顺序为（参考资料而来，还未实际验证，试试就知道了，不必拘泥，仅供参考）：

首先匹配 `=`，其次匹配`^~`, 其次是按文件中顺序的正则匹配，最后是交给 `/` 通用匹配。当有匹配成功时候，停止匹配，按当前匹配规则处理请求。

#### `rewrite`

    rewrite regex replacement [flag]

- regex：被代替的原 URL 路径，可以是莫须有的，不存在的，支持正则表达式

与 js 中正则比较，缺少了首`/`和尾`/`，比如，匹配`/fe/`开头的 url，匹配的正则为：

    /^\/fe\/(._?)\$/

此处则需要写为：

    ^\/fe\/(._?)$

可以补充首尾`/`后通过 js 验证正则的正确性，比如直接在浏览器 F12 下的 console 里进行：

    /^\/fe\/(.*?)$/.test('/fe/abc')

- replacement：用来实现代替的 URL 路径，必须真实存在的

中间可以使用`$1`，`$2` 来匹配 Regex 匹配结果分别对应，同样可以在浏览器 console 中运行：

    '/fe/abc'.match(/^\/fe\/(.*?)$/)

返回结果为：

    ["/fe/abc","abc"]

则`$1` 即为`"abc"`

- flag：标志位，定义 URL 重写后进行的操作，有 4 种，分别是：

  - `last`:匹配重写后的 URL，再一次对 URL 重写规则进行匹配，当使用`last`的需要注意的是如下：

        rewrite /images/.\*\.jpg /images/a.jpg last;

    这样写的话，将会造成死循环。

  - `break`：匹配重写 URL 后，终止匹配，直接使用

  - `redirect`：临时重定向，返回代码 302

  - `permanent`：永久重定向，返回代码 301

根据以上参数，`80`的`/fe/`重写到`localhost:7000/v1/`上，则可以：

```conf
location ^~ /fe/ {
  rewrite ^\/fe\/(.*?)$ /v1/$1 last;
}
location ^~ /v1/ {
  proxy_pass localhost:7000;
}
```

### 常见问题

#### 框架`browserHistory`

```conf
location / {
    try_files $uri $uri/ /index.html;
}
```

#### 代理修改 IP 问题

经过代理后，服务端拿到的 IP 会是代理 nginx 的 IP，而非原始请求的原始 IP，可以：

```conf
location ^~ /fe/ {
  proxy_pass http://127.0.0.1:6000;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

来获取原始 IP。

#### 伪静态

其实就是将请求原始请求`/url/12`改为`/url/12.html` 通过`rewrite`重写将请求转到原始路径上：

```conf
location ~* \/url\/([1-9]+).html$ {
  rewrite ^(\/url\/)([0-9]+)\.html$ $1$2 last;
  return 404;
}
location ~* \/url/(([1-9]+)?)$ {
  proxy_pass http://127.0.0.1:6000;
}
```
