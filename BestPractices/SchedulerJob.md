# 定时任务最佳实践

## 更新日志

* 2018-4-2: 首次书写本文档   钟云昶

## 简介

定时任务是做为biz-framework的一项重要模块，借助系统自带的定时任务功能，可以让网校在后台执行一些后台任务，本文档基于Edusoho主程序，展示一些使用上的最佳实践

## 引入定时任务组件

```php
//biz.php
$biz->register(new \Codeages\Biz\Framework\Provider\SchedulerServiceProvider());
```

## 定时任务配置项





