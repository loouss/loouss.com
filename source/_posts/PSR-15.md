---
title: PSR-15
date: 2020-07-19 15:20:26
tags:
---


# PSR 15 	HTTP请求处理器

### 主要接口

- RequestHandlerInterface 请求处理器

  ```php
  namespace Psr\Http\Server;
  
  use Psr\Http\Message\ResponseInterface;
  use Psr\Http\Message\ServerRequestInterface;
  
  /**
   * 处理服务器请求并返回响应
   *
   * HTTP 请求处理程序处理 HTTP 请求，以便生成 HTTP 相应。
   */
  interface RequestHandlerInterface
  {
      /**
       * 处理服务器请求并返回响应
       *
       * 可以调用其他协助代码来生成响应。
       */
      public function handle(ServerRequestInterface $request): ResponseInterface;
  }
  ```

- MiddlewareInterface  中间件

  ~~~php
  namespace Psr\Http\Server;
  
  use Psr\Http\Message\ResponseInterface;
  use Psr\Http\Message\ServerRequestInterface;
  
  /**
   * 参与处理服务器的请求与响应
   *
   * 一个 HTTP 中间件组件参与处理一个 HTTP 的消息:
   * 通过对请求进行操作, 生成相应,或者将请求转发给后续的中间件，并  且可能对它的响应进行操作
   * 
   */
  interface MiddlewareInterface
  {
      /**
       * 处理一个传入的请求
       *
       * 处理传入的服务器请求以产生相应.
       * 如果无法生成响应本身，它可能会委托给提供的请求处理程序来执行此操作
       * 
       */
      public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface;
  }
  ~~~


### 简单实现示例（伪代码）

- RequestHandlerInterface 请求处理器

  ~~~php
  class HttpRequestHandler implements RequestHandlerInterface
  {
      
      protected $middlewares = [];
      
      protected $offset = 0;
      
      public function handle(ServerRequestInterface $request): ResponseInterface
      {
          $middleware = $this->middlewares[$this->offset];
          $middleware->process($request, $this->next());
      }
      
      protected function next(): self
      {
          ++$this->offset;
          return $this;
      }
  }
  ~~~

- MiddlewareInterface  中间件

  ~~~php
  class FormatMiddleware implements MiddlewareInterface
  {
      public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface
      {
          return $handler->handle($request);
      }
  }
  
  ~~~

  > 请求处理器与中间互相调用，直至在中间件中返回 ***ResponseInterface*** 实例。

