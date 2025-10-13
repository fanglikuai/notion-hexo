---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WODLBJSX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDr%2BvHVJ6j5GFuHrmLZEJfTlqCz0ZBrXI%2F%2FhG6KMG0bQAiBYSJS5L%2BC5T3qkVnVu0bN1VMtQwVfuAENe3Phf9Xnjlyr%2FAwhMEAAaDDYzNzQyMzE4MzgwNSIMj%2FaIE9%2BB8IbWeo%2FvKtwDTlMXmRYH%2B%2BF%2BzU9HMFnzrTaKeBp7cdI4Sj5koXtRF5a1P3mNwbw2hrg%2FjantSGE%2FXC6%2FS936%2BBmG5uX79avafke1z4l0iIyeDrFa4WjPOZMKorymRQnwckCvCI%2FIBM3dexugGr9GH9s20TV9rNUjhDjAaic3viCOc0zw8yXm1Mj9k29SK0utvCSN0sEfdFcb8I87wn7T32WCp8NwmfN4Gf%2BOrj73CUbtr6abXQTHU27UFskBpo0MImjALO4FUCcp7PpZyUAVjYKPR%2FbpDNKOAwvmbNMPe%2FnzmbW4OUwOCfwXQF9D0Hpw0KdUrs%2F36sEgSHiXHerWUvK5KCeHA9UvS6vJSIR0ohFmHzlAY42gPu9elIB5XIFZJ8deuCLuzictZMO%2Btedhtqp57fOmsD9xq1Ov4IM7V6lJVBoG5M6UmrkTMYN54djsBCbzMSG%2BnncIyrzdzT5sQZFKF2aAJBBYPfE68gRYWjKKZXShhe9d51%2F4GGCwHqfJc9r5p%2BKpY2Z7vS8jptahsbyIhyNX7tBgrahJ2SGF6wKgJLi7bzqqa1S3XTMwrbDz5EDLkpElG%2BSTciq4VG8w%2BkcrDYdoW%2B9APmnjT6gBX%2FQv9qYNexSZIDbjLOk1OD5DM1vnnqowpZC1xwY6pgFs0ZIS%2FtCPAnJN6p8I28ofS8KgS%2BmbE1%2FPhFjapCdaKdvnmF0X%2BZJwfWqxefd4lp85LfeBYfGWMf2qEKmMivO4SQUFRnI7kSiFRrC%2B51w9vDDoZKh%2Fa1X4qzYapTpKH3oSH9%2BtBq0%2BazgJFYvHcrhGbvXp%2B24xv2UUBId%2FoD7G9%2Bn6eFJ1lDiDTClzd4wM0%2B%2FvuGHDcURzdETNzHB4M%2FIcmH3LnX8O&X-Amz-Signature=6b4fbf32da50fc5bda4192b24b04d9ebd050cef56b848e6227c01a072a3f1a4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
