---
title: docker运行yapi
date: 2020-08-10 14:13:44
tags:
---


# 首先启动mongo

~~~shell

sudo docker run --detach --restart  always --name mongo-yapi --volume /mnt/mongo/data/db:/data/db mongo

~~~

> yapi数据存储在mongo数据库中
> --volume 路径根据自己实际情况填写

# 初始化yapi及查看密码

~~~shell

sudo docker run -it --rm --link mongo-yapi:mongo --entrypoint npm --workdir /api/vendors registry.cn-hangzhou.aliyuncs.com/anoy/yapi run install-server

~~~

# 启动yapi

~~~shell

 sudo docker run --detach --restart  always --name yapi --link mongo-yapi:mongo --workdir /api/vendors --publish 127.0.0.1:3000:3000 registry.cn-hangzhou.aliyuncs.com/anoy/yapi server/app.js

~~~




