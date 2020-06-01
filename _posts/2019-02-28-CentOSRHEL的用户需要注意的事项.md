---
layout: post
title: "CentOS/RHEL的用户需要注意的事项"
date: 2019-02-28
excerpt: ""
tags: [docker]
comments: true
---

​	在Ubuntu/Debian上有UnionFS可以使用，如aufs或者overlay2，而CentOS和RHEL的内核中没有相关驱动。因此对于这类系统，一般使用devicemapper驱动利用LVM的一些机制来模拟分层存储。这样的做法除了性能比较差外，稳定性一般也不好，而且配置相对复杂。Docker安装在CentOS/RHEL上后，会默认选择devicemapper，但是为了简化配置，其devicemapper是跑在一个稀疏文件模拟的块设备上，也被称为loop-lvm。这样的选择是因为不需要额外配置就可以运行Docker，这是自动配置唯一能做到的事情。但是loop-lvm的做法非常不好，其稳定性、性能更差，无论是日志还是docker info中都能看到警告信息。官方文档有明确的文章讲解了如何配置块设备给devicemapper驱动做存储层的做法，这类做法也被称为配置direct-lvm。

​	除了前面说到的问题，devicemapper+looplvm还有一个缺陷，因为它是稀疏文件，所以它会不断增长。用户在使用过程中会注意到`/var/lib/docker/devicemapper/devicemapper/data`不断增长，而且无法控制。很多人会希望删除镜像或许可以解决这个问题，结果发现效果并不明显。原因就是这个稀疏文件的空间释放后基本不进行垃圾回收的问题。因此往往会出现即使删除了文件内容，空间却无法回收，随着使用这个稀疏文件一直在不断增长。

​	所以对于CentOS/RHEL的用户来说，在没有办法使用UnionFS的情况下，一定要配置direct-lvm给devicemapper，无论是为了性能、稳定性还是空间利用率。

​	或许有人注意到了CentOS7中存在被backports回来的overlay驱动，不过CentOS里的这个驱动达不到生产环境使用的稳定程度，所以不推荐使用。