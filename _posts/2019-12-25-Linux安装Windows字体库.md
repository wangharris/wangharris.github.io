---
layout: post
title: "Linux安装Windows字体库"
date: 2019-12-25
excerpt: ""
tags: [linux]
feature: https://harriswang.gitee.io/myimagehosting/img/wallpaper/2019-11-01.jpeg
comments: true
---

### 发行版

elementary os

### 环境

win10 + eos 双系统

### 安装步骤

1. 确认Linux下Windows的C盘的挂载目录，我的是在`/media/harris/Windows/`。

2. 复制windows的字体库到linux下：

   ```shell
   cd /usr/share/fonts
   sudo mkdir WindowsFonts
   sudo cp /media/harris/Windows/Windows/Fonts/* WindowsFonts/
   sudo fc-cache
   ```

3. 此时，windows字体库就安装成功了，可以打开WPS或者ThunderBird进行确认。

