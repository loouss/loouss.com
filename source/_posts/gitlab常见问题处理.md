---
title: gitlab常见问题处理
date: 2020-08-17 17:05:45
tags:
---


# 丢失二次验证令牌

~~~shell
//[来源这里](https://docs.gitlab.com/ee/security/two_factor_authentication.html#disabling-2fa-for-everyone)
sudo gitlab-rake gitlab:two_factor:disable_for_all_users

gitlab-rails runner 'User.find_each(&:disable_two_factor!)'
~~~

> 关闭所有用户二次验证

# 重启gitlab，或者密钥文件丢失

> 删除项目web页面500错误，ci/cd 页面500错误。

## 方法一

~~~shell
gitlab-rails dbconsole
gitlabhq_production=> SELECT name, runners_token_encrypted FROM Projects WHERE Name = 'projectName';
~~~

> 根据项目名称查询数据库中存储的token，在页面去更新。

## 方法二 

~~~shell
root@gitlab:/# gitlab-rails dbconsole
psql (11.7)
Type "help" for help.

gitlabhq_production=> UPDATE projects SET runners_token = null, runners_token_encrypted = null; 
UPDATE 17
gitlabhq_production=> UPDATE namespaces SET runners_token = null, runners_token_encrypted = null;
UPDATE 9
gitlabhq_production=>  UPDATE application_settings SET runners_registration_token_encrypted = null;
UPDATE 1
gitlabhq_production=> 
~~~

> 此方法会重置有所有项目的token，谨慎操作。

## 方法三

~~~shell
gitlab-rails dbconsole
gitlabhq_production=> UPDATE Projects SET runners_token = null, runners_token_encrypted = null WHERE name = 'cicd-test';
UPDATE 1
~~~

> 重置单个项目**token**

# SSH 无法连接

~~~shell
λ git pull
Connection closed by xx.xx.xx.xx port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
~~~

## 解决方法
删除**/etc/gitlab**下的所有文件
> 注意删除之前备份文件
