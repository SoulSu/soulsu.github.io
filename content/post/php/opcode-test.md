---
title: "php 开启 opcode 测试"
date: 2018-04-06T17:10:39+08:00
draft: false
author: "soulsu"
tags: ["php"]
---


php 开启 opcode 测试,合理使用～

<!--more-->

---


#### 实验环境

* 系统信息 `Linux localhost.localdomain 3.10.0-514.10.2.el7.x86_64 #1 SMP Fri Mar 3 00:04:05 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux`

* 内存 `512M` CPU `1核`
* PHP 版本 `PHP 7.0.21 ` & `Zend OPcache v7.0.21`

####  opcode 介绍

1. php 运行方式

    1. Scanning(Lexing) ,将PHP代码转换为语言片段(Tokens)
    2. Parsing, 将Tokens转换成简单而有意义的表达式
    3. Compilation, 将表达式编译成Opocdes
    4. Execution, 顺次执行Opcodes，每次一条，从而实现PHP脚本的功能。

2. Zend 中opcode 缓存结构体

```c
struct _zend_op {
    opcode_handler_t handler; // 执行该opcode时调用的处理函数
    znode result; // 函数返回信息
    znode op1; // 参数1
    znode op2; // 参数2
    ulong extended_value;
    uint lineno;
    zend_uchar opcode;  // opcode代码
};
```

3. 参数配置参考地址 [点击这里](http://php.net/manual/zh/opcache.configuration.php#ini.opcache.save-comments)


4. opcode 注意事项

    1. opcode 生成规则是，通过时间戳进行生成新 opcode，**在生产环境中如果发布版本回退，老的opcode 生成时间会大于回退版本文件的当前时间戳的。** 也就是说不会再更新啦。



#### 测试结果

测试代码
```php
<?php
for($i=0;$i<100;$i++){
    echo "Hello Opcache";
}
```

> 执行命令 `ab -n 1000 -c 100 http://192.168.1.110:8080/t1.php`

开启时测试结果结果

```
Concurrency Level:      100
Time taken for tests:   0.976 seconds
Complete requests:      1000
Failed requests:        0
Write errors:           0
Total transferred:      1430000 bytes
HTML transferred:       1300000 bytes
Requests per second:    1024.27 [#/sec] (mean)
Time per request:       97.630 [ms] (mean)
Time per request:       0.976 [ms] (mean, across all concurrent requests)
Transfer rate:          1430.38 [Kbytes/sec] received

```

关闭后测试结果

```
Concurrency Level:      100
Time taken for tests:   1.695 seconds
Complete requests:      1000
Failed requests:        0
Write errors:           0
Total transferred:      1430000 bytes
HTML transferred:       1300000 bytes
Requests per second:    589.89 [#/sec] (mean)
Time per request:       169.522 [ms] (mean)
Time per request:       1.695 [ms] (mean, across all concurrent requests)
Transfer rate:          823.78 [Kbytes/sec] received

```


#### 结论

> 在开启 `opcache` 后 每秒钟请求从 `589.89` 增长到了 `1024.27`
合理利用 `opcache` 会给程序带来不错的优化效果

