---
title: 在linux中安装docker
date: 2018-06-05 11:06:40
tags: docker
---

## 安装docker

- #### CentOS

   ```shell
   $ sudo yum install yum-utils device-mapper-persistent-data lvm2
   $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   $ sudo yum install docker-ce
   $ sudo systemctl enable docker
   $ sudo systemctl start docker
   ```

- #### Debian

  ```shell
  $ sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
  $ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
  $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
  $ sudo apt-get update
  $ sudo apt-get install docker-ce
  $ sudo systemctl enable docker
  $ sudo systemctl start docker
  ```

- #### Fedora

  ```shell
  $ sudo dnf -y install dnf-plugins-core
  $ sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
  $ sudo dnf install docker-ce
  $ sudo systemctl enable docker
  $ sudo systemctl start docker
  ```
- #### Ubuntu
  ```shell
  $ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  $ sudo apt-get update
  $ sudo apt-get install docker-ce
  $ sudo systemctl enable docker
  $ sudo systemctl start docker
  ```
