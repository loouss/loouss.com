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

# 提换或者丢失**/etc/gitlab**目录下密钥后，postgresql数据库中存储的token失效

> 删除项目web页面500错误，ci/cd 页面500错误。

## 方法一

~~~shell
gitlab-rails dbconsole
gitlabhq_production=> SELECT name, runners_token_encrypted FROM Projects WHERE Name = 'projectName';
~~~

> 根据项目名称查询数据库中存储的token，在页面去更新。

## 方法二 

~~~shell
gitlab-rails dbconsole
gitlabhq_production=> UPDATE projects SET runners_token = null, runners_token_encrypted = null;
UPDATE 11
gitlabhq_production=> UPDATE namespaces SET runners_token = null, runners_token_encrypted = null;
UPDATE 23
gitlabhq_production=> UPDATE application_settings SET runners_registration_token_encrypted = null;
UPDATE 6
~~~

> 此方法会重置有所有项目的token，谨慎操作。