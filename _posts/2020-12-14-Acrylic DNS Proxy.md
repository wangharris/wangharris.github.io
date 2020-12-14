---
layout: post
title: "Acrylic DNS Proxy"
date: 2020-12-14
excerpt: ""
tags: [network]
feature: https://harriswang.gitee.io/myimagehosting/img/wallpaper/2019-10-26.jpeg
comments: true
---



　　[Acrylic](http://mayakron.altervista.org/support/acrylic/Home.htm)是一款Windows下的DNS代理软件，我安装它原因是它在配置hosts文件方面支持通配符，这样我就不用每次在新增一个二级域名的时候都去修改hosts文件。

　　安装好软件后，打开`Acrylic UI`客户端，打开`File -> Open Acrylic Configation`可以修改公共DNS服务器地址：

```
PrimaryServerAddress=114.114.114.114
```

　　打开`File -> Open Acrylic Hosts`添加域名解析：

```
172.16.2.35 *.xlck.com
```

　　保存，会自动重启Acrylic Service。

　　打开网络设置，设置DNS服务器地址为`127.0.0.1`，重启电脑。

　