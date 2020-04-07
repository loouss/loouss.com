---

title: hyperf源码分析
date: 2020-04-05 14:53:49
tags: php
---



## hyperf源码分析

- #### 流程

  1. 入口文件创建container
  2. 初始化container时调用 DefinitionSourceFactory 返回 DefinitionSource对象
  3. DefinitionSourceFactory __invoke() 读取框架配置，实例化 ScanConfig::class
  4. 实例化 DefinitionSource::class 
  5. 在 DefinitionSource::class 构造方法中 调用 scan() 扫描文件并加载元数据，添加ast，收集已扫描文件的注释并存储。
  6. 在 DefinitionSource::class 构造方法中 调用 normalizeSource() 方法存储类的 DefinitionInterface 的实例。
  7. 入口文件调用container get方法获取 ApplicationFactory class ，在ApplicationFactory类中添加 commond 返回symfony/console 组件 Application 示例
  8. 根据控制台输入命令启动相应模块

- #### 入口文件

  ```php

  ...
  require BASE_PATH . '/vendor/autoload.php';
  
  // Self-called anonymous function that creates its own scope and keep the global namespace clean.
  (function () {
      /** @var \Psr\Container\ContainerInterface $container */
      $container = require BASE_PATH . '/config/container.php';
  
      $application = $container->get(\Hyperf\Contract\ApplicationInterface::class);
      $application->run();
  })();
  ```

1.  require 引入一个container 文件并创建容器（容器遵循psr11）

2. 调用container->get()   获取Application 实例（framework组件）

3. symfony/console 启动

   > ##### 注意：
   >
   > ```
   > 所有组件下 ConfigProvider.php 文件中的依赖声明
   > ```

- #### 获取Container及DefinitionSource部分

  ```php
  ...
  $container = new Container((new DefinitionSourceFactory(true))());
  
  if (! $container instanceof \Psr\Container\ContainerInterface) {
      throw new RuntimeException('The dependency injection container is invalid.');
  }
  return ApplicationContext::setContainer($container);
  ```

1. 实例化 DefinitionSourceFactory 类并执行该类 __invoke() 方法，初始化框架加载配置，返回  DefinitionSource 实例。

   ```php
   Hyperf\Di\Definition\DefinitionSourceFactory
   ...
   加载配置文件内容
   ...
   $scanConfig = new ScanConfig($scanDirs, $ignoreAnnotations, $collectors);
   
   return new DefinitionSource($serverDependencies, $scanConfig, $this->enableCache);
   ```

2.   Hyperf\Di\Definition\DefinitionSource

   ```php
   public function __construct(array $source, ScanConfig $scanConfig, bool $enableCache = false)
   {
       //实例化Scanner 并设置忽略注解名称
       $this->scanner = new Scanner($scanConfig->getIgnoreAnnotations());
    $this->enableCache = $enableCache;
   
       // $scanConfig “Hyperf\Di\Definition\DefinitionSourceFactory 类中读取配置文件值部分”
       // Scan the specified paths and collect the ast and annotations.
       $this->scan($scanConfig->getDirs(), $scanConfig->getCollectors());
       // 存储$definitions
       $this->source = $this->normalizeSource($source);
   }
   ...
   // $paths $collectors  “Hyperf\Di\Definition\DefinitionSourceFactory 类中读取配置文件值部分”
   private function scan(array $paths, array $collectors): bool
   {
       $appPaths = $vendorPaths = [];
   
       /**
        * If you are a hyperf developer
        * this value will be your local path, like hyperf/src.
        * @var string
        */
       $ident = 'vendor';
       $isDefinedBasePath = defined('BASE_PATH');
   
       foreach ($paths as $path) {
           if ($isDefinedBasePath) {
               if (Str::startsWith($path, BASE_PATH . '/' . $ident)) {
                   $vendorPaths[] = $path;
               } else {
                   $appPaths[] = $path;
               }
           } else {
               if (strpos($path, $ident) !== false) {
                   $vendorPaths[] = $path;
               } else {
                   $appPaths[] = $path;
               }
           }
       }
   
       $this->loadMetadata($appPaths, 'app');
       $this->loadMetadata($vendorPaths, 'vendor');
   
       return true;
   }
   ...
   private function loadMetadata(array $paths, $type)
   {
       if (empty($paths)) {
           return true;
       }
       $cachePath = $this->cachePath . '.' . $type . '.cache';
       $pathsHash = md5(implode(',', $paths));
       //hash 和 文件更新时间判断缓存是否有效 缓存有效则不需要扫描文件
       if ($this->hasAvailableCache($paths, $pathsHash, $cachePath)) {
           $this->printLn('Detected an available cache, skip the ' . $type . ' scan process.');
           [, $serialized] = explode(PHP_EOL, file_get_contents($cachePath));
           //收集类，属性，方法的注释到 Hyperf\Di\Annotation\AnnotationCollector::$container中
           $this->scanner->collect(unserialize($serialized));
           return false;
       }
       $this->printLn('Scanning ' . $type . ' ...');
       $startTime = microtime(true);
       //实际执行扫描文件 加载文件类容
       $meta = $this->scanner->scan($paths);
       foreach ($meta as $className => $stmts) {
           AstCollector::set($className, $stmts);
       }
       $useTime = microtime(true) - $startTime;
       $this->printLn('Scan ' . $type . ' completed, took ' . $useTime * 1000 . ' milliseconds.');
       if (! $this->enableCache) {
           return true;
       }
       // enableCache: set cache
       if (! file_exists($cachePath)) {
           $exploded = explode('/', $cachePath);
           unset($exploded[count($exploded) - 1]);
           $dirPath = implode('/', $exploded);
           if (! is_dir($dirPath)) {
               mkdir($dirPath, 0755, true);
           }
       }
   
       $data = implode(PHP_EOL, [$pathsHash, serialize(array_keys($meta))]);
       file_put_contents($cachePath, $data);
       return true;
   }
   ```