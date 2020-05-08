---
title: composer
date: 2020-05-08 15:00:39
tags:
---

## 简介

Composer 是 PHP 的一个依赖管理工具。它允许你申明项目所依赖的代码库，它会在你的项目中为你安装他们。

## 安装composer

下载地址: https://mirrors.aliyun.com/composer/composer.phar

- LIKE UNIX ：

```shell
mv composer.phar /usr/local/bin/composer
```

- WIN ：

设置系统的环境变量PATH。
在 composer.phar 同级目录下新建文件 composer.bat :

```shell
C:\bin>echo @php "%~dp0composer.phar" %*>composer.bat
```

- 验证安装

```shell
composer -v
```

## 包版本

在前面的例子中，我们引入的 monolog 版本指定为 `1.0.*`。这表示任何从 `1.0` 开始的开发分支，它将会匹配 `1.0.0`、`1.0.2` 或者 `1.0.20`。

版本约束可以用几个不同的方法来指定。

| 名称         | 实例                                    |                             描述                             |
| :----------- | :-------------------------------------- | :----------------------------------------------------------: |
| 确切的版本号 | `1.0.2`                                 |                   你可以指定包的确切版本。                   |
| 范围         | `>=1.0` `>=1.0,<2.0` `>=1.0,<1.1|>=1.2` | 通过使用比较操作符可以指定有效的版本范围。 有效的运算符：`>`、`>=`、`<`、`<=`、`!=`。 你可以定义多个范围，用逗号隔开，这将被视为一个**逻辑AND**处理。一个管道符号`|`将作为**逻辑OR**处理。 AND 的优先级高于 OR。 |
| 通配符       | `1.0.*`                                 | 你可以使用通配符`*`来指定一种模式。`1.0.*`与`>=1.0,<1.1`是等效的。 |
| 赋值运算符   | `~1.2`                                  | 这对于遵循语义化版本号的项目非常有用。`~1.2`相当于`>=1.2,<2.0`。 |

> ~ (波浪号运算符) ，``~1.2`` 相当于 `` >=1.2,<2.0``

## composer.lock 锁文件

在安装依赖包时，composer 会把安装时确切的版本号写入`composer.lock` 文件。

通常情况下，需要提交 `composer.lock` 文件，因为 `install` 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本。

这意味着，任何人建立项目都将下载与指定版本完全相同的依赖。你的持续集成服务器、生产环境、你团队中的其他开发人员、每件事、每个人都使用相同的依赖，从而减轻潜在的错误对部署的影响。即使你独自开发项目，在六个月内重新安装项目时，你也可以放心的继续工作，即使从那时起你的依赖已经发布了许多新的版本。

如果不存在 `composer.lock` 文件，Composer 将读取 `composer.json` 并创建锁文件。

如果你需要更新依赖包，可以执行 `composer update` 命令，这将获取最新匹配的版本更新（根据 `composer.json` 文件）同时也会更新 `composer.lock` 文件。



## 自动加载

安装依赖包时，composer 会生成 `vendor/autoload.php` 文件，引入这个文件即可实现自动加载。

自定义命名空间加载也可以在 `composer.json` 的 `autoload` 字段中增加自己的命名空间与文件映射。

```json
{
    "autoload": {
        "psr-4": {"App\\": "app/"}
    }
}
```

composer 将注册一个 [PSR-4](http://www.php-fig.org/psr/psr-4/) autoloader 到  `App` 命名空间。

## composer 配置

#### 环境变量 

- `COMPOSER_HOME` 

    默认值：

    - like unix `/home//.composer`

    - OSX `/Users//.composer`

    - Windows `C:\Users\AppData\Roaming\Composer`

    可以在 `COMPOSER_HOME`  目录中配置  `config.json` 文件，在执行 ` composer update ` 或者 `composer install` 命令时，`composer` 会与项目中的 `composer.json` 文件进行合并。如果存在相同配置项，项目中的 `composer.json` 文件优先级更高。

- `COMPOSER_CACHE_DIR`

    默认值：

    - like unix `$COMPOSER_HOME/cache`

    - Windows `C:\Users\\AppData\Local\Composer` 或 `%LOCALAPPDATA%/Composer`

    - 也可以通过 `cache-dir` 进行配置

      ```json
      {
          "config": {
              "cache-dir": "D:\\php\\Composer\\cache"
          }
      }
      ```

      > 也可以将上面的上面的 `json` 文件添加到 `$COMPOSER_HOME/config.json` 文件中

      

> [更多环境变量](https://getcomposer.org/doc/03-cli.md#environment-variables)