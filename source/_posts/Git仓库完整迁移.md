---
title: Git仓库完整迁移
date: 2020-05-15 14:41:37
tags: git
---



# Git仓库完成迁移

完整迁移：迁移仓库内所有commit

1. 将git仓库`clone`到本地
```shell

git clone --bare https://xxx.xxx.git

```

2. 进入仓库文件夹`push`到新的仓库地址
```shell

git push --mirror https://xxx.xxx.git

```

> 仓库地址为自己项目中实际地址