---
title: RabbitMQ极简入门
date: 2020-04-25 12:34:34
tags: RabbitMQ
---

## 单机部署

#### 安装docker
 - [在linux中安装docker](https://loouss.gitee.io/2018/06/05/%E5%9C%A8linux%E4%B8%AD%E5%AE%89%E8%A3%85docker/)

#### docker运行RabbitMQ
```shell
$ sudo docker run --detach --name rabbitmq --publish 15672:15672 \
 --publish 5672:5672 
 --volume /home/vagrant/rabbitmq:/var/lib/rabbitmq \ 
 --env RABBITMQ_NODENAME=rabbit@localhost rabbitmq:3.8.3-management-alpine
```

- 查看RabbitMQ运行状态

  ```shell
  $ sudo docker exec rabbitmq rabbitmqctl status
  
  Status of node rabbit@localhost ...
  Runtime
  
  OS PID: 164
  OS: Linux
  Uptime (seconds): 41914
  RabbitMQ version: 3.8.3
  Node name: rabbit@localhost
  Erlang configuration: Erlang/OTP 22 [erts-10.7] [source] [64-bit] [smp:2:2] [ds:2:2:10] [async-threads:64]
  Erlang processes: 438 used, 1048576 limit
  Scheduler run queue: 1
  Cluster heartbeat timeout (net_ticktime): 60
  
  Plugins
  
  Enabled plugin file: /etc/rabbitmq/enabled_plugins
  Enabled plugins:
  
   * rabbitmq_management
   * rabbitmq_web_dispatch
   * rabbitmq_management_agent
   * amqp_client
   * cowboy
   * cowlib
  
  Data directory
  
  Node data directory: /var/lib/rabbitmq/mnesia/rabbit@localhost
  
  Config files
  
   * /etc/rabbitmq/rabbitmq.conf
  
  Log file(s)
  
   * <stdout>
  
  Alarms
  
  (none)
  
  Memory
  
  Calculation strategy: rss
  Memory high watermark setting: 0.4 of available memory, computed to: 0.8359 gb
  other_proc: 0.035 gb (29.44 %)
  code: 0.0302 gb (25.35 %)
  other_system: 0.024 gb (20.17 %)
  allocated_unused: 0.0195 gb (16.34 %)
  plugins: 0.0031 gb (2.57 %)
  other_ets: 0.0029 gb (2.41 %)
  atom: 0.0015 gb (1.27 %)
  binary: 0.0013 gb (1.11 %)
  mgmt_db: 0.0011 gb (0.95 %)
  metrics: 0.0002 gb (0.18 %)
  mnesia: 0.0001 gb (0.07 %)
  connection_other: 0.0 gb (0.04 %)
  quorum_ets: 0.0 gb (0.04 %)
  msg_index: 0.0 gb (0.02 %)
  queue_procs: 0.0 gb (0.01 %)
  connection_readers: 0.0 gb (0.01 %)
  connection_channels: 0.0 gb (0.0 %)
  connection_writers: 0.0 gb (0.0 %)
  queue_slave_procs: 0.0 gb (0.0 %)
  quorum_queue_procs: 0.0 gb (0.0 %)
  reserved_unallocated: 0.0 gb (0.0 %)
  
  File Descriptors
  
  Total: 3, limit: 1048479
  Sockets: 1, limit: 943629
  
  Free Disk Space
  
  Low free disk space watermark: 0.05 gb
  Free disk space: 47.9716 gb
  
  Totals
  
  Connection count: 1
  Queue count: 1
  Virtual host count: 1
  
  Listeners
  
  Interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
  Interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
  Interface: [::], port: 15672, protocol: http, purpose: HTTP API
  
  ```

#### Management Plugin 插件简单使用

使用浏览器，访问 http://127.0.0.1:15672/ 地址，我们就可以看到 RabbitMQ Management 的登陆界面。默认情况下，我们可以使用用户名为 `guest` ，密码为 `guest` 进行登陆。

##### 控制面板基本概念

- Override：整体概览
- Connections：**网络连接**，比如一个 TCP 连接
- Channels：**信道**，多路复用连接中的一条独立的双向数据流通道，信道是建立在真实的 TCP 连接内地虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接
- Exchanges：**交换器**，用来接收生产者发送的消息并将这些消息路由给服务器中的队列
- Queues：**消息队列**，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走

~~~
注意：一定记得修改"guest"账号
~~~

## PHP 简单示例

#### composer 引入依赖

~~~json
composer require php-amqplib/php-amqplib
~~~

- ## RabbitMQProducer

~~~php
<?php

require_once __DIR__ . '/vendor/autoload.php';
use PhpAmqpLib\Connection\AMQPStreamConnection;
use PhpAmqpLib\Message\AMQPMessage;

//创建连接
$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
//创建通道
$channel = $connection->channel();
//声明队列
$channel->queue_declare('hello', false, false, false, false);

$msg = new AMQPMessage('Hello World!');
//发布消息
$channel->basic_publish($msg, '', 'hello');

echo " [x] Sent 'Hello World!'\n";
//关闭通道
$channel->close();
//关闭连接
$connection->close();
?>
~~~


- ## RabbitMQConsumer

~~~php
<?php

require_once __DIR__ . '/vendor/autoload.php';
use PhpAmqpLib\Connection\AMQPStreamConnection;

//创建连接
$connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
//创建通道
$channel = $connection->channel();
//声明队列
$channel->queue_declare('hello', false, false, false, false);

echo " [*] Waiting for messages. To exit press CTRL+C\n";
//消息回调函数
$callback = function ($msg) {
    echo ' [x] Received ', $msg->body, "\n";
};

$channel->basic_consume('hello', '', false, true, false, false, $callback);

while ($channel->is_consuming()) {
    $channel->wait();
}

$channel->close();
$connection->close();
?>
~~~

> 创建从大到小，关闭从小到大

