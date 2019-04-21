---
title: CentOS（1） — 虚拟机安装
date: 2019-04-14 13:15:21
tags: [Linux, CentOS]
category: 'linux'
---

最近工作中总需要和服务器打交道，还都是 CentOS6.9 的系统，本想找运维申请一个，但是后来感觉服务器用的不熟悉，运维同学帮我出个主意，让我自己装个虚拟机，然后虚拟网络映射一下就可以让其他人也能访问了（公司内网 IP 和 mac 地址绑定，虚拟机用不了 NAT 模式），就可以自己随便搞，搞坏了再开一个虚拟机就可以了。

### 安装

- 宿主机：Windows 10
- 软件：VMware Workstations Pro
- 虚拟机系统镜像：CentOS-6.9-x86_64-minimal.iso

安装过程就不多说，只需要点击下一步。

### 基本设置

安装出来是一个最简服务器系统。

#### 设置静态 IP

1. **编辑文件**

```shell
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

配置改为：

```ini
DEVICE=eth0
HWADDR=00:0C:29:A6:79:C1
TYPE=Ethernet
UUID=a0ea8ec6-88d4-407c-aac2-3b6b4572a129
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.234.24
NETMASK=255.255.255.0
GATEWAY=192.168.234.2
DNS1=114.114.114.114
DNS2=202.103.24.68
```

其中的 IP、网关和 DNS 都需要根据实际需要进行配置。

如果使用 VMware 需要注意其中的网关设置，可能并非常见的“192.168.\*.1”，我之前一度配置错误，知道后来才注意到：VMware –> 编辑 -> 虚拟网络编辑器 -> 选择 NAT 一行 -> 点击 NAT 设置（如果遇到不能编辑情况，是因为需要管理权限，可以在其右下角找到按钮获取权限），这里可以看到需要的网关。

![VMware 虚拟网络编辑器](/assets/articles/img/vmware-gateway.png 'WMware 网关')

2. **测试网络**

```shell
$ ping www.baidu.com
```

能够正常联网后，通过 Xshell 连接改主机进行后续配置（Xshell 连接后配置更方便）。

### 安装 `wget`

```shell
$ yum install wget
```

#### 修改 `yum` 源到阿里云

1. **备份本地源**

```shell
$ mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

2. **安装阿里源**

下载新的`CentOS-Base.repo`到`/etc/yum.repos.d/`。

CentOS 5：

```shell
$ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
```

CentOS 6：

```shell
$ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
```

CentOS 7：

```shell
$ wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

### 其他问题

#### 关闭防火墙

临时关闭防火墙：

```shell
$ servcie iptables stop
```

永久关闭防火墙：

```shell
$ chkconfig iptables off
```

#### 局域网访问

虚拟机使用了 NAT，处于一个更小的局域网，宿主机所在局域网主机访问不到其上端口或服务，但是可能会需要与其他人共享，此时需要对[宿主机进行设置](/linux/centos/centos-vm-install/)
