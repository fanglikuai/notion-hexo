---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UEE7AHP%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T142021Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAUE0XQL%2BZOrHEcg2IxTWDz41IlSOOeH6TIj%2BJaj%2FGyRAiEA4lfmJp2OUatNeduasTbsRwQNFBA4%2FBmTmQE6m3OkWQQq%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDGz%2F4l3OFeFeu70zJyrcAwtrlgikzcImovJfPeCYN31MvTpValdgTcjCydPWTQOlvPnTDR5CmAzm9LoYA2XpwI0UDsnvsJiBBxgtB%2FaXl2j5wam9yR6%2BCKPnFIrks%2Fs47dF8eMUrEltFkFJFkraNQTkkeME2Ddbd6T6NQQ94N8vFPV4tmnwGPxckBfe4mIBq27dyYUrOxkSgIXkcegOJh2NY3h8GYdqRISXQSrYtjJz2q%2FS%2BLcYFyj3flw0iGnM5%2FUHsIelnSfBJ93sOPAeeshddheT%2B2BKmsP%2F8r0fY%2FdV36rm2EoWDkchSKGata4oKQSyhGKZvbbWBS9abSJ5sYHsHtEBw5Y2OovDLmnwHW2M3MAnIGLvr7ONvf%2BHR5tYH0Sr5BLyItT7pe1NJT6W9qLUD7Rsaeq7TkcV2Botea5qJv7aScnASw1%2FzmFu2KNCZXjmlT7%2FH6EJHFMw3QAKrIJmVDE9VzqooSRUXiZtRvX4qeg4S5Q1Cl7hfTdqKm%2Fqm%2FmktXuvvcjZIsgSC92FIJmJchEfR4eZlXJ%2FFAnH2rE6MHK4utPttm0mSA8gqsYiV3Vt40d7cd064wG8qGWG0jcjxeLuaYuKoUrkAxMTqlgf1dtWH%2BiEl%2BzbOVZOnkPmWTSmW4v6tsiGPGADFMO61oMYGOqUBkfeNxqvZ0p2b%2F9HVs9elMMe0IB%2FiNcAtu4R%2FC00%2BomoS12HIKpA7cUu8RogZf8r8bO%2FShUB1kDJIUlNOURbo8onzBBru7UC5olU1x04WaLiTsiuJBwTMRNXEEuww44Pl9FBUz4ssxmHQp1C26NbYZcmATB%2Bs6BjuNpQy%2FxQhEiuGsBHA%2Bf8RbvOjHTEBs6s3BNXmBjC2J%2FJvc1zpVJ5HR4LnnOrW&X-Amz-Signature=5e483bfd96348b4b6f9d25c13ebac2122e1158d494e6164a05b2f1777def1e36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
index_img: /images/681caddd167c86081c93eb4da2dc581a.png
banner_img: /images/681caddd167c86081c93eb4da2dc581a.png
---

# 基本概念


**Nginx (engine x)** 是一款轻量级的 Web 服务器 、反向代理服务器及电子邮件（IMAP/POP3）代理服务器。


**反向代理与正向代理的区别：**


正向代理：在用户这一端，vpn


反向代理：在服务器端，nginx

> 拓展：
>
> 堡垒机：统一的运维入门，带权限认证
>
>

基本使用：


```bash
nginx -s stop
#快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。nginx -s quit
#平稳关闭Nginx，保存相关信息，有安排的结束web服务。nginx -s reload
#因改变了Nginx相关配置，需要重新加载配置而重载。nginx -s reopen
#重新打开日志文件。nginx -c filename
#为 Nginx 指定一个配置文件，来代替缺省的。nginx -t
#不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。nginx -v
#显示 nginx 的版本。nginx -V
#显示 nginx 的版本，编译器版本和配置参数。
```


# 实战


反向代理域名的tomcat


```plain text
upstream zp_server1{
  server 127.0.0.1:8080;
  # 写要代理的地方
}
server {
  listen       80;
  server_name  www.helloworld.com; #从哪里来的域名

  #charset koi8-r;

  #access_log  logs/host.access.log  main;

  location / {
    #  root   html;
    # index  index.html index.htm;
    proxy_pass http://zp_server1;
    #进行代理
  }
```


## 跨域问题

1. 在 Nginx 的`server` 或`location`块中添加以下头部：

```plain text
location / {
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
  add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
  add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';

  # 处理预检请求 OPTIONS
  if ($request_method = 'OPTIONS') {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
    add_header 'Access-Control-Max-Age' 1728000;
    add_header 'Content-Type' 'text/plain; charset=utf-8';
    add_header 'Content-Length' 0;
    return 204;
  }

  # 其他请求正常处理
  ...
}
```

1. 指定的域名可以跨域
