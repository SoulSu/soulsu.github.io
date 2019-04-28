---
title: "Goroutine Pool 实现"
date: 2018-04-06T17:17:37+08:00
draft: false
author: "soulsu"
tags: ["Golang"]
---


该代码块是阅读 TiDB 项目

<!--more-->

---


> 原项目代码 [地址](https://github.com/pingcap/tidb/blob/master/util/goroutine_pool/gp.go)


#### 设计方法

1. 通过链表的形式维护多个协程
2. 每个内部协程都有个死循环等待任务传入
3. 复用协程-> 能有效节约程序调用切换，节约系统创建协程的开销


{{% gist "SoulSu" "addadf131ab2b71229d51d94f1ce8357" %}}

