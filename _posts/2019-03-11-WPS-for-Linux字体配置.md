---
layout: post
title: "WPS for Linux字体配置"
date: 2019-03-11
excerpt: "系统缺失字体解决方案"
tags: [linux]
comments: true
---

1. 问题现象

   ![](https://harriswang.gitee.io/myimagehosting/img/post/linux/wps-fonts.png)

2. 原因

   > 目前WPS for Linux公式显示需要相应的Symbol字体（比如symbol, windings, mt extra等）， 由于版权原因，WPS for Linux未对此类字体打包安装，如果您需要，请在授权的情况下使用此类字体。

3. 解决方案

   - 下载字体库

     链接: [https://pan.baidu.com/s/1XQYRQMRV6niIu1zpKlUpUw](https://pan.baidu.com/s/1XQYRQMRV6niIu1zpKlUpUw) 提取码: 3sew 

   - 解压

     ```shell
     sudo unzip wps_symbol_fonts.zip -d /usr/share/fonts/wps-office
     ```

   - 重新打开WPS