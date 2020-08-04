---
title: Jenkins简单示例
date: 2020-08-03 22:07:24
tags:
---

# Java 项目

新建任务时项目类型选择 ***构建一个maven项目*** 

![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/jenkins4.png)

## 构建项目配置 

- **General 部分**

  填写项目信息，设置构建参数等

- **源码管理**

  可以选择 ***git***,***svn***

  - git 填入仓库地址及访问密钥（Credentials），指定分支（branch）

    ![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/jenkins1.png)

- **构建触发器**

  可以用git push event触发Jenkins构建，或者定时器构建等

  ![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/jenkins2.png)

- **构建环境**

  没有特殊需求可以不用理会

- **Pre Steps** 

  构建前操作，可以执行shell脚本等根据实际情况处理

- **Build**

  pom.xml文件路径

- **Post Steps**

  构建完成时操作

  > 触发前置条件：Run only if build succeeds , Run only if build succeeds or is unstable , Run regardless of build result
  >
  > 如果需要push docker image，可以见选择 ***Run only if build succeeds*** 只在构建成功时执行shell完成镜像push

  示例shell

  ~~~shell
  REGISTRY="ccr.ccs.tencentyun.com/loouss/xxxx"
  docker build -t $REGISTRY:$GIT_COMMIT .
  docker push $REGISTRY:$GIT_COMMIT
  docker rmi $REGISTRY:$GIT_COMMIT
  ~~~



# PHP项目

新建任务时项目类型选择 ***构建一个自由风格的软件项目***

![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/jenkins5.png)

## 构建项目配置 

- **General**

  与java项目同理

- **源码管理**

  与java项目同理

- **构建触发器**

  与java项目同理

- **构建环境**

  与java项目同理

- **构建**

  构建步骤可以根据实际项目选择，简单示例可以如下

  ![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/jenkins6.png)

  ~~~shell
  REGISTRY="ccr.ccs.tencentyun.com/loouss/xxxx"
  docker build -t $REGISTRY:$GIT_COMMIT .
  docker push $REGISTRY:$GIT_COMMIT
  docker rmi $REGISTRY:$GIT_COMMIT
  ~~~

- **构建后操作**

  可以根据实际情况选择执行

  ![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/jenkins7.png)