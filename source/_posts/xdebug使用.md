---
title: xdebug使用
date: 2020-10-06 09:23:18
tags:
---


# xdebug安装

> [xdebug](https://xdebug.org/)官网

- Linux & Macs
  ~~~shell
  pecl install xdebug
  ~~~
  修改**php.ini**文件
  ~~~
  zend_extension="/xxx/xxx/xdebug.so"
  ~~~
- windows
  [下载](https://xdebug.org/download)对应版本

  修改**php.ini**文件
  ~~~
  zend_extension="/xxx/xxx/xdebug.dll"
  ~~~

# xdebug配置

### 远程调试
 - **boolean** xdebug.auto_trace = false
  >  启用改配置，将在脚本调用前启用追踪。cli模式下需要启用改配置，以开启追踪。
 - **string** xdebug.remote_addr_header = ""
  > 配置header与**xdebug.remote_connect_back** 一起使用
 - **boolean** xdebug.remote_connect_back = false
  > 检查 **$_SERVER** 数组中变量
 - **boolean** xdebug.remote_autostart = false
  > 启用改配置，xdebug将始终尝试远程调试会话开始
 - **integer** xdebug.remote_cookie_expire_time = 3600
  > 会话保持时间
 - **boolean** xdebug.remote_enable = false
  > 是否连接一个远程调试客户端
 - **string** xdebug.remote_handler = dbgp
  > 调试协议，目前仅支持**dbgp**
 - **string** xdebug.remote_host = localhost
  > 调试客户端地址
 - **integer** xdebug.remote_port = 9000
  > 调试客户端监听端口

### 性能分析
 - integer xdebug.profiler_aggregate = 0
  > 设置值为1，开启聚合统计
 - integer xdebug.profiler_append = 0
  > 设置值为1，文件不会被覆盖
 - integer xdebug.profiler_enable = 0
  > 设置值为1，开起性能分析
 - integer xdebug.profiler_enable_trigger = 0
  > 设置值为1，开起性能触发器
 - string xdebug.profiler_enable_trigger_value = ""
  > 触发器匹配的值
 - string xdebug.profiler_output_dir = /tmp
  > 分析文件输出的文件目录
 - string xdebug.profiler_output_name = cachegrind.out.%p
  > 文件名称


> 更多及详情配置请参考[文档](https://xdebug.org/docs/all_settings)

# 基本原理

如果使用phpstorm，配置**dbgp proxy**

![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/20201007213730.png)

在php.ini配置xdebug remote信息

![](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/20201007220952.png)

开启了xdebug.auto_trace配置的话，可以不添加触发方式（如谷歌浏览器插件xdebug-helper，在cookie中添加XDEBUG_*），只需要在ide中添加断点即可，这种方式也适用调试CLI方式运行的脚本。

> zendvm检测开启了xdebug，会判断是否需要触发条件，有触发条件，连接远程调试客户端，判断是否有断点，有断点则停止在断点处，调试客户端发出指令（执行下一行代码或者跳过函数等）继续执行。


