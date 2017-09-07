---
layout:     post
title:      "将Solr5添加到windows服务"
subtitle:   ""
date:       2016-01-27
author:     "itcamel"
tags:
    - solr
---

Solr从5.0版本开始，发行包中不再包含solr.war这样的文件，也就不能直接在tomcat之类的容器内执行，取而代之的是Solr内嵌的jetty容器。
然而jetty并不能像tomcat那样很方便的添加到windows服务，这里我用到了一个小工具：[NSSM](https://nssm.cc)。通过它可以快速的将Solr的启动注册为windows服务。

- 首先下载NSSM工具，地址：[https://nssm.cc/download](https://nssm.cc/download)

- 解压下载包，并在解压目录执行如下命令：

    ```powershell
    "c:\Program Files\nssm\win64\nssm" install solr5
    ```

- 在打开的对话框中填入solr.cmd路径及执行参数。

    ![NSSM service editor](http://7xqkz3.com1.z0.glb.clouddn.com/@/image/posts/nssm006.png)
    > 在设置参数时一定要加上`-f`（foreground），否则服务会反复启停。

- 在参数都填写完以后，点击`install service`按钮即可添加服务。
