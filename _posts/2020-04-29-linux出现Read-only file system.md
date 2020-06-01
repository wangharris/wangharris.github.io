---
layout: post
title: "Linux出现Read-only file system"
date: 2020-04-29
excerpt: ""
tags: [linux]
feature: https://harriswang.gitee.io/myimagehosting/img/wallpaper/2019-10-25.jpeg
comments: true
---



在停电或者电脑死机后强制关机导致文件系统受损，重启电脑后，受损分区就会被Linux自动挂载为只读，提示Read-only file system。

解决方法是通过fsck来修复文件系统，然后重启即可，以下是以针对/dev/xvde1分区，ext4文件系统分区的一个操作案例

```shell
fsck.ext4 -y /dev/xvde1
```



**要针对出问题的分区进行操作，在挂载了多个硬盘的机器上要仔细分辨一下。**