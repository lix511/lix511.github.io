---
layout:     post
title:      "lvs的负载调度算法"
subtitle:   ""
date:       2016-01-26
author:     "itcamel"
tags:
    - lvs
---

LVS是个负载均衡设备，它不提供任何服务，用户请求到这里的时候，它是将客户需求转发至后端真正提供服务的服务。  
LVS的十种调度算法如下：

#### 1. 大锅饭调度(Round-Robin Scheduling RR)
> `rr`: 纯轮询方式。把每项请求按顺序在真正服务器中分派。
轮询调度算法假设所有服务器的处理性能都相同，不关心每台服务器的当前连接数和响应速度，当请求服务间隔时间变化比较大时，轮询调度算法容易导致服务器间的负载不平衡。
所以此种均衡算法适合于服务器组中的所有服务器都有相同的软硬件配置并且平均服务请求相对均衡的情况。

#### 2. 带权重的大锅饭调度(Weighted Round-Robin Scheduling WRR)
> `wrr`: 带权重轮询方式。把每项请求按顺序在真正服务器中循环分派，但是给能力较大的服务器分派较多的作业。
由于权重轮询调度算法考虑到了不同服务器的处理能力，所以这种均衡算法能确保高性能的服务器得到更多的使用率，避免低性能的服务器负载过重。所以，在实际应用中比较常见。

#### 3. 谁不干活就给谁分配(Least-Connection LC)
> `lc`: 根据最小连接数分派

#### 4. 带权重的谁不干活就给谁分配（Weighted Least-Connections WLC 默认）
> `wlc`: 带权重的。机器配置好的权重高。

#### 5. 基于地区的最少连接调度(Locality-Based Least-Connection Scheduling LBLC)
> `lblc`: 缓存服务器集群。基于本地的最小连接。把请求传递到负载小的服务器上。

#### 6. 带有复制调度的基于地区的最少连接调度(Locality-Based Least-Connection Scheduling with Replication Scheduling LBLCR)
> `lblcr`: 带复制调度的缓存服务器集群。某页面缓存在服务器A上，被访问次数极高，而其他缓存服务器负载较低，监视是否访问同一页面，如果是访问同一页面则把请求分到其他服务器。

#### 7. 目标散列调度(Destination Hash Scheduling DH)
> `dh`: realserver中绑定两个ip。ld判断来者的ISP商，将其转到相应的IP。

#### 8. 源散列调度(Source Hash Scheduling SH)
> `sh`: 源地址散列。基于client地址的来源区分。（用的很少）

#### 9. 最短的期望的延迟（Shortest Expected Delay Scheduling SED）
> `sed`: 基于wlc算法。举例来说：
ABC三台机器分别权重123 ，连接数也分别是123。那么如果使用WLC算法的话一个新请求进入时它可能会分给ABC中的任意一个。使用sed算法后会进行这样一个运算
A:(1+1)/1
B:(1+2)/2
C:(1+3)/3
根据运算结果，把连接交给C。

#### 10. 最少队列调度（Never Queue Scheduling NQ）
> `nq`: 无需队列。如果有台realserver的连接数＝0就直接分配过去，不需要在进行sed运算。
