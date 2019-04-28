---
title: "优雅的shell参数编程"
date: 2018-04-12T09:48:33+08:00
draft: false
author: "soulsu"
tags: ["shell", "linux"]
---

当自己写shell脚本的时候，会使用到自定义的参数，如何更加方便的接收参数值是一个很重要的问题；安利一个好用的方法，使用 getopts 来获取参数和参数值～

<!--more-->

---

#### getopts

1. 查看帮助： `man shell` 然后搜索 `getopts`
2. 输入参数： `getopts optstring name [args]`
3. **:** 分割表示后面需要带上参数值
4. "a:b:c" 参数a,b 后面需要带上值
5. 匹配成功后会内置两个变量；`OPTIND`表示参数偏移量，`OPTARG`表示参数对应的值





使用方法 

```bash
#!/bin/bash


a="default a"
b="default b"
c="default c"

# OPTERR=0
while getopts "a:b:c:h" opt
do

    case $opt in
        a)
            echo "a ${OPTIND} ${OPTARG}"
            a=${OPTARG}
        ;;
        b)
            echo "b ${OPTIND} ${OPTARG}"
            b=${OPTARG}
        ;;
        c)
            echo "c ${OPTIND} ${OPTARG}"
            c=${OPTARG}
        ;;
        h)
            echo "${opt} used ${0} [-a|-b|-c]"
        ;;
    esac

done

echo $a
echo $b
echo $c

```

当输入 `-c true -a 123 -b "arg b"` 的时候会覆盖变量默认值并输出
```
c 3 true
a 5 123
b 7 arg b
123
arg b
true
```