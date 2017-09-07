---
layout:     post
title:      "Linux环境下solr的服务模式安装"
subtitle:   ""
date:       2017-09-07
author:     "itcamel"
tags:
    - linux
    - solr
---

Solr 5.0以后的版本，内置Jetty Server，部署运行比原来简单的多，安装包解压以后`solr start`就可以运行。  
当然，生产环境需要安装为服务，更利于运维。而服务的安装也是非常简单，将安装包中的 `/bin/install_solr_service.sh` 解压出来，然后执行它即可。

需要注意这个命令的参数：
```
Usage: install_solr_service.sh path_to_solr_distribution_archive OPTIONS

  The first argument to the script must be a path to a Solr distribution archive, such as solr-5.0.0.tgz
    (only .tgz or .zip are supported formats for the archive)

  Supported OPTIONS include:

    -d     Directory for live / writable Solr files, such as logs, pid files, and index data; defaults to /var/solr

    -i     Directory to extract the Solr installation archive; defaults to /opt/
             The specified path must exist prior to using this script.

    -p     Port Solr should bind to; default is 8983

    -s     Service name; defaults to solr

    -u     User to own the Solr files and run the Solr process as; defaults to solr
             This script will create the specified user account if it does not exist.
```