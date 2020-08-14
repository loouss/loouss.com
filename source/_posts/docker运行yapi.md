---
title: docker运行yapi
date: 2020-08-10 14:13:44
tags:
---


# ??????

~~~shell

sudo docker run -it --rm --link mongo-yapi:mongo --entrypoint npm --workdir /api/vendors registry.cn-hangzhou.aliyuncs.com/anoy/yapi run install-server

~~~

> ???yapi????????????

# ??mongo

~~~shell

sudo docker run --detach --restart  always --name mongo-yapi --volume /mnt/mongo/data/db:/data/db mongo

~~~

# ??yapi

~~~shell

 sudo docker run --detach --restart  always --name yapi --link mongo-yapi:mongo --workdir /api/vendors --publish 127.0.0.1:3000:3000 registry.cn-hangzhou.aliyuncs.com/anoy/yapi server/app.jssud

~~~

