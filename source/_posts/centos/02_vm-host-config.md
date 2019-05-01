---
title: 宿主机所在局域网访问虚拟机端口/服务
date: 2019-04-18 13:15:21
tags: [Linux, CentOS, VMware]
category: 'linux'
---

我们会在虚拟机上部署 web 服务、数据库服务等，希望局域网内其他人也能够一起访问，但是虚拟机使用了 NAT，处于一个更小的局域网，宿主机所在局域网主机访问不到。

### VMware 添加端口映射

去虚拟机找到：VMware –> 编辑 -> 虚拟网络编辑器，得到上图；然后添加“端口转发”。

![VMware 虚拟网络编辑器](/img/vmware-gateway.png 'WMware_网关')

### 宿主机添加防火墙规则

添加后，宿主机局域网内其他主机通过宿主机的 IP+转发端口还是不能访问通，这里就是因为防火墙问题了，需要添加“入站规则”来达到。

1. 通过控制面板进入防火墙设置：控制面板/所有控制面板项/Windows Defender 防火墙

![Windows Defender 防火墙](/img/Windows%20Defender%20%E9%98%B2%E7%81%AB%E5%A2%99.png 'Windows Defender 防火墙')

2. 选择高级设置，点击添加规则

![Windows Defender 防火墙 - 高级设置](/img/Windows%20Defender%20%E9%98%B2%E7%81%AB%E5%A2%99%20-%20%E9%AB%98%E7%BA%A7%E8%AE%BE%E7%BD%AE.png 'Windows Defender 防火墙 - 高级设置')

3. 选择规则类型 - 端口

![入站规则 - 规则类型](/img/%E5%85%A5%E7%AB%99%E8%A7%84%E5%88%99%20-%20%E8%A7%84%E5%88%99%E7%B1%BB%E5%9E%8B.png '入站规则 - 规则类型')

4. 输入需要对外开放的端口（端口映射中的宿主机端口）

![入站规则 - 输入端口](/img/%E5%85%A5%E7%AB%99%E8%A7%84%E5%88%99%20-%20%E8%BE%93%E5%85%A5%E7%AB%AF%E5%8F%A3.png '入站规则 - 输入端口')

5. 一直点下一步，最后输入规则名、描述信息，完成规则添加。
