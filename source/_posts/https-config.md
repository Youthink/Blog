---
title: 如何给网站添加免费的 https 证书
date: 2019-11-27
updated: 2019-11-27
tags:
categories: 编程
---

{% qnimg https-config.jpg 'alt:https' %}

最近部署了一个新服务，想给域名启用 `https`，结果搜了半天，在我的博客里只发现了这一篇笔记：[阿里云免费证书域名启用 https](https://hufangyun.com/2017/https-aliyun-free/)

😹 只能说给网站启用 `https` 太简单了，之前都没留下笔记，为了之后方便，简单记录一下。

我使用的是 [Let’s Encrypt](https://letsencrypt.org/) 的免费证书。

按照下面的操作一步步来就行了。

1、去 [certbot](https://certbot.eff.org/) 官网。

2、在页面上选择一下你用的 `Web` 服务器和操作系统。比如，我选的 `Nginx` 和 `Ubuntu 18.04`

3、页面上会出现操作步骤，按照提示的步骤一步步执行命令就可以了。

需要注意执行到 `sudo certbot --nginx` 的时候，会出现两个协议：

第一个，是问你要不要同意他们的服务协议，选择“同意”。

第二个，是问你要不要共享你的邮箱给他们，他们偶尔会发一些他们基金会的新闻一类的。我选了“不同意”（因为我不知道以后有没有退订选项 😂）。

之后 `certbot` 会自动扫描你的 `Nginx` 的配置，寻找域名列表然后让你选择想配置 `https` 证书的域名（好像可以同时配置多个域名）。

如果没找到域名的话，会让你手动输入一个域名。

之后会问你，要不要强制把 `http` 的链接跳转到 `https` 上，当然选“是”了。

之后 `certbot` 会帮你把 `Nginx` 一切都配置好。你直接访问域名看一下效果即可。服务真是相当的贴心。。。。🎉

耗叔的这篇文章 [如何免费的让网站启用HTTPS|酷 壳 - CoolShell](https://coolshell.cn/articles/18094.html) 提到配置`http2` 传输性能更好。

> 要求 `Nginx` 版本要大于 `1.9.5`。`HTTP2` 具有更快的 `HTTPS` 传输性能，非常值得开启。需要开启 `HTTP/2` 其实很简单，只需要在 `nginx.conf` 的 `listen 443 ssl;` 后面加上 `http2` 就好了。如下所示：

```nginx
listen 443 ssl http2; # managed by Certbot 
ssl_certificate /etc/letsencrypt/live/coolshell.cn/fullchain.pem; # managed by Certbot 
ssl_certificate_key /etc/letsencrypt/live/coolshell.cn/privkey.pem; # managed by Certbot 
include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
```
然后，`nginx -s reload` 就好了。

