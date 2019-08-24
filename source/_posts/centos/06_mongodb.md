---
title: mongodb 安装与配置
date: 2019-08-24 10:59:29
tags: [Linux, CentOS, mongodb]
category: 'linux'
---

[mongo_office]: https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/

最近开始做一个 nodejs 项目，查阅了好久资料来判断使用哪个数据库，最后选择了 mongodb。由于当时服务器还需要跑 docker，一下都是基于 CentOS7.5 进行的操作。

其实其中有好些都是根据[官方文档][mongo_office]来做的，但是还是有一部分感觉不理解，查了一些资料做了以下总结。

### 安装

    $ vi /etc/yum.repos.d/mongodb-org-4.0.repo

添加以下内容：

```conf
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
```

    $ yum install mongodb-org

启动、设置开机启动：

    $ systemctl start mongod
    $ systemctl enable mongod

### 配置

#### 备份原始配置文件

曾经配置错好多次，但是又找不到原始配置文件，再也启动不起来了，费好大力气才重头开始配置

    $ cp /etc/mongod.conf /etc/mongod.conf.backup

#### 修改配置

    $ vi /etc/mongod.conf

修改内容有：

```yml
systemLog:
  destination: file
  logAppend: true
  # 日志 配置自己希望的路径
  path: /data/mongo/log/mongod.log

# ···

# Where and how to store data.
storage:
  # 数据 配置自己希望的路径
  dbPath: /data/mongo/data/
  journal:
    enabled: true

# ···

# network interfaces
net:
  port: 27017
  # 端口、IP 默认127.0.0.1，仅机器内部可访问
  bindIp: 0.0.0.0 # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.
```

#### 新建文件夹

新建以上配置的数据、日志路径，如示例的路径：

    $ mkdir -p /data/mongo/data/
    $ mkdir -p /data/mongo/log/

#### 文件访问权限

给新建文件夹添加 mongo 权限，确保 mongodb 可以正常访问。

    $ chown -R mongod:mongod /data/mongo/

### 创建登录用户

#### 首先重启

创建用户前需要重启数据库，保证数据存储在了以上设置的路径中，不然添加完用户后重启，用户数据就丢失了（重启前后数据存储路径不同）。

    $ systemctl restart mongod

创建用户目的是开启数据库用户登录，只有输入对应的用户密码才能访问数据库。

#### 配置管理员

**注：**以下所使用的账号和密码，都可以根据自己的需要来自己添加，我这里仅仅是示例。

进入数据库（由于没有开启账号密码登录，可以直接进入数据库）：

    $ mongo

创建超级管理员：

    $ use admin
    $ db.createUser({ user: "admin", pwd: "<your own password>", roles: [{ role: "userAdminAnyDatabase", db: "admin" }]})

创建不受限制超级用户:

    $ db.createUser({ user: "root", pwd: "<your own password>", roles: ["root"] })

#### 开启权限验证

退出数据库，修改配置，开启权限验证：

    $ vi /etc/mongod.conf

```yml
security:
  authorization: enabled # 启用账号密码登录
```

#### 配置数据库用户

重新登录数据库，已经开启权限验证，此次需要对应的账号密码登录：

    $ mongo –u admin –p

当然，也可以使用一些软件来连接服务器（以上`bindIP`已经修改），这个怎么使用本文档不再描述，仅记录服务器使用方式。

创建数据库：

    $ db.createCollection("test-db")

其实数据库账号有两种方式：

1. 在`admin`数据里添加用户，配置访问某个数据库权限；
2. 在数据库里添加自己的用户

这也就是使用一些工具或代码连接中，需要输入一个 Auth DB 的原因。

其中我使用的是第二种：

    $ use test-db
    $ db.createUser({ user:"testUser", pwd:"<your own password>", roles:[{role: "readWrite", db: "test-db"}] })

其实两种的不同也就是这个用户创建到哪个数据库而已，然后需要用的 Auth DB 跟着变化即可。

此处已经可以开始通过你的账号名与密码来使用创建的数据库啦。
