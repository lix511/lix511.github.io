---
layout:     post
title:      "CentOS系统下MySQL更改数据文件路径"
subtitle:   ""
date:       2016-01-26
author:     "itcamel"
tags:
    - MySQL
    - CentOS
---

MySQL在实际生产环境中，当安装路径的磁盘分区较小时，通常需要更改数据文件的存储路径。

1. 停止MySQL服务

    ```powershell
    service mysqld stop
    ```

2. 移动MySQL默认数据库文件

    ```powershell
    mv /var/lib/mysql /home/
    ```

3. 修改MySQL配置文件

    ```powershell
    vi /etc/my.cnf
    ```
    > datadir=`/var/lib/mysql`改为`/home/mysql`  
    socket=`/var/lib/mysql/mysql.sock`改为`/home/mysql/mysql.sock`

4. 做一个mysql.sock的链接

    ```powershell
    mkdir /var/lib/mysql
    ln -s /home/mysql/mysql.sock /var/lib/mysql/mysql.sock
    ```

5. 最后重启MySQL服务

    ```powershell
    service mysqld start
    ```