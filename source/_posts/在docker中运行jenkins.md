---
title: 在docker中运行jenkins
date: 2020-04-04 21:52:43
tags: jenkins
---



## Jenkins安装

- docker

```
docker run \
-u root \
--rm \
-d \
-p 8080:8080 \
-p 50000:50000 \
-v jenkins-data:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
jenkinsci/blueocean
```
> eg:
>
> sudo docker run -u root --name jenkins --rm -d -p 8081:8080 -p 127.0.0.1:50000:50000 -v /mnt/jenkins/data:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -e JAVA_OPTS=-Duser.timezone=Asia/Shanghai jenkinsci/blueocean



>  安装后安装插件 ， 和更新Jenkins，可以在启动是 添加环境变量设置时区。 具体细节可以看 Dockerfile 文件。
> 安装插件可能会多次连接超时，可以先升级jenkins版本或者更换插件源。
> container 重启后，maven构建缓存会丢失，Jenkins升级会失效，可以通过外部 war 包挂载进container的方式管理jenkins版本，容器里的所有状态会重置为镜像初始状态。
> 可以将docker.sock 挂载进入container ，jenkins拥有处理docker接口的能力

  