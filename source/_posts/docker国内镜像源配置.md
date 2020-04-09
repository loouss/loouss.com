---
title: docker国内镜像源配置
date: 2020-04-09 14:14:30
tags: docker
---

- #### linux

  在 Linux 环境下，我们可以通过修改 `/etc/docker/daemon.json` ( 如果文件不存在，可以直接创建它 ) 这个 Docker 服务的配置文件达到效果。

  ```json
  {
      "registry-mirrors": [
          "https://registry.docker-cn.com"
      ]
  }
  ```
  在修改文件之后重启 docker daemon

  ```shell
  sudo systemctl restart docker
  ```
  重启之后可以通过 docker info 来查看当前镜像列表

​      