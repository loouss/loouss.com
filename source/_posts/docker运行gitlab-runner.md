---
title: docker运行gitlab-runner
date: 2020-08-17 17:58:41
tags:
---


# 注册**runner**，生成配置文件

## 从本地指定目录注册

~~~shell
docker run --rm -it -v /application/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register
~~~

> **/application/gitlab-runner/config**本地目录路径

## 使用**volumes**注册

~~~shell
docker run --rm -it -v gitlab-runner-config:/etc/gitlab-runner gitlab/gitlab-runner register
~~~


# 启动**runner**

## 从本地指定目录启动

~~~shell
docker run -d --name gitlab-runner --restart always -v /application/gitlab-runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner
~~~

> **/application/gitlab-runner/config**本地目录路径

## 使用**volumes**启动

~~~shell
docker volume create gitlab-runner-config

docker run -d --name gitlab-runner --restart always -v /var/run/docker.sock:/var/run/docker.sock -v gitlab-runner-config:/etc/gitlab-runner gitlab/gitlab-runner:latest
~~~

# 非交互式注册

~~~shell
docker run --rm -it -v /application/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register \
  --non-interactive \
  --url "http://127.0.0.1:8080/" \
  --registration-token "tc-noUPFsBsxhfzo68o2" \
  --executor "docker" \
  --docker-image alpine:latest \
  --description "runner" \
  --tag-list "docker,aws" \
  --run-untagged \
  --locked="false"
~~~

> 上面命令中请替换你实际的 **url** 和 **token**