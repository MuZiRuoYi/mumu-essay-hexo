---
title: GitHub Pages 自定义域名使用 HTTPS 协议
date: 2018-06-03 15:36:44
tags: [GitHubPages]
category: 'GitHubPages'
---

[github_custom_domain]: https://help.github.com/articles/quick-start-setting-up-a-custom-domain/
[zhuhu_reason]: https://zhuanlan.zhihu.com/p/22667528
[cloudflare]: https://dash.cloudflare.com/

不知道从什么时候开始，自己搭建的 github pages 的协议不再是 HTTPS 协议，查了一些资料，发现是自定义域名的原因，Github 官方给出了[Setting up a custom domain][github_custom_domain]，就查找了一些**免费**的解决方案，并且已经亲测成功。

我也是按照[教程][zhuhu_reason]搞出来的，给一个[Cloudflare][cloudflare]地址，自己玩吧。

不再多说具体教程，我就是记录一下自己的理解吧：

说直白点就是找个 DNS 服务商，将域名商提供的预设 DNS 服务器修改为自己找的其他 DNS 服务商提够的 DNS 服务器地址。

1.  寻找 DNS 服务商
2.  获取 DNS 服务，并且一定不要忘记使用 HTTPS，服务商会给出 DNS 服务器地址（Cloudflare 是两个）
3.  进入域名服务商 DNS 管理（我用的是 GoDaddy，不知道其他是不是同样套路）
4.  修改 NameServers，将预设改为 DNS 服务商给出的服务器地址

然后去忙其他的吧（设置后有延时），什么时候想起这回事（我是第二天睡醒想起来的）再去自己 GitHub Pages 看看，应该好了。
