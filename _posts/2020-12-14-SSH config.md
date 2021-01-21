---
layout: post
title: "SSH config"
date: 2020-12-14
excerpt: ""
tags: [network]
feature: https://harriswang.gitee.io/myimagehosting/img/wallpaper/2019-10-01.jpeg
comments: true
---



　　SSH config是SSH客户端的一个参数配置文件，所在路径为`~/.ssh/config`。



示例文件：

```
Host gitlab.xlck.com
    HostName gitlab.xlck.com
    User harris-wang@qq.com
    IdentityFile /c/Users/harris/.ssh/id_rsa_company
    
Host git.code.tencent.com
    HostName git.code.tencent.com
    User harris-wang@qq.com
    IdentityFile /c/Users/harris/.ssh/id_rsa_home

Host github.com
    HostName github.com
    User harris-wang@qq.com
    IdentityFile /c/Users/harris/.ssh/id_rsa_home

Host gitee.com
    HostName gitee.com
    User harris-wang@qq.com
    IdentityFile /c/Users/harris/.ssh/id_rsa_home
```



参数类型：

- Host

  类似昵称，标识某个主机的配置。

- HostName

  主机的域名或IP。

- User

  主机的登录用户名

- IdentityFile

  认证证书文件，默认是`~/.ssh/id_rsa`，即私钥。
