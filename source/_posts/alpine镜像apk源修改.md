---
title: alpine镜像apk源修改
date: 2020-08-04 23:25:11
tags:
---


alpine 原生apk源网络原因，国内访问延迟较高，可以替换为阿里云镜像

~~~shell
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
~~~