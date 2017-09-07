---
layout:     post
title:      "Linux系统中JAVA进程CPU占用率高的问题排查"
subtitle:   ""
date:       2016-01-28
author:     "itcamel"
tags:
    - solr
---

### 问题描述
> 生产环境下的某台tomcat7服务器，在刚发布时的时候一切都很正常，但运行一段时间后就出现CPU占用很高的问题，基本上是负载一天比一天高。

### 问题分析
- 程序属于CPU密集型，和开发沟通过，排除此类情况。
- 程序代码有问题、出现死循环的可能性极大。

### 故障点定位
1. 根据top命令，发现PID为2633的Java进程占用CPU极高。

2. 找到该进程后，显示线程列表，并按照CPU占用高的线程排序：
```shell
ps -mp 2633 -o THREAD,tid,time | sort -rn
```
显示结果为：
> 
USER     %CPU PRI SCNT WCHAN  USER SYSTEM   TID     TIME  
root     10.5  19    - -         -      -  3626 00:12:48  
root     10.1  19    - -         -      -  3593 00:12:16  
**找到了耗时最高的线程3626，占用CPU时间有12分钟了！**

3. 将需要的线程ID转换为16进制格式：
```shell
printf "%x\n" 3626
```
显示结果为：
> e18

4. 打印线程的堆栈信息：
```shell
jstack 2633 |grep e18 -A 30
```

5. 通过线程的堆栈信息就基本可以找到代码的问题所在了。
