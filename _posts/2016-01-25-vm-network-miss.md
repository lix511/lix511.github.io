---
layout:     post
title:      "vsphere部署ovf模板（CentOS）后网卡失效"
subtitle:   ""
date:       2016-01-25
author:     "itcamel"
tags:
    - vsphere
    - CentOS
---

## 问题描述

用vsphere client部署ovf模板（CentOS操作系统）以后，网卡失效，`service network start`輸出信息如下：

```powershell
Shutting down loopback insterface: [ OK ]
Bringing up loopback insterface: [ OK ]
Bringing up interface eth0: Device eth0 does not seem to be present,delaying initialization. [FAILED]
```

## 解决办法

1. 删除网卡配置并重启。

    ```powershell
    rm -rf /etc/udev/rules.d/70-persistent-net.rules
    reboot
    ```

2. 修改MAC地址及IP地址。
    
    ```powershell
    vi /etc/sysconfig/network-scripts/ifcfg-eth0
    ```
    > 将`HWADDR`，`IPADDR`改成实际的MAC地址及IP地址。

3. 重启网络服务即可。

    ```powershell
    service network restart
    ```