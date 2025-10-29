---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPQBENZJ%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIB0Gzh%2BGTibuCtoMdoitemnITY3CDIM7bVPufStkHaSNAiB4dLD4iAbkY4EjHmxR46r%2FPijl8uLey2ctni2Xziaf2SqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMz%2FrmW2VQFDYPau8FKtwDscgLVM1OVM%2BDsb2WvVMugqZekvaQcpr0Hu14IkTxt2LJZxq3P3cMZUQuTx3x50ZFQY2zKTcGY4ChfBhDPsKvmAO5oYQ1n17h2pb1uST9qMq97%2BrMu3dHztrV4BD3qQADP8oy4idNazeiB4uNROVXxpgnpYeaOzHML11kwbqdyv7JmzZbOErwpBAumeFC8INZ5Lybs6X%2ByqP%2F9MfqQFPCj9E8ecA0gbwpLgDIXKnz7IZNYFghjSzzSfnZt%2BZ0rDtcEUVTQG5gTJsMGVMKTIdOQ9HF%2BIbYhGiAntmcqXwuG4XgJGNJ7dbQ%2FcjjgQA5Cq2NYmOPHtMPYmK0ZMb0WTaJMifih39B8EfeukQYBrorJrDboPjlbF85C9NCOyppOwfEpmxTrYjUGOdzKg%2B4GQBVpdyOG6rvlZp8c3qkL6gIxNdHFiXl0jcGeZC1Kk9A8G8DsuTICsPGbSYMgiulSfn6SJyDm9%2BXo7xWZRWP98mh0EcWEoya%2FYClVVa4hqsoAgmhHuHU4WA0%2FnOcsBgSrm1YIfmAUpTbOsEADarwxKDy64GHoQ9sQXds43plEd2bFOz%2BNG7c%2FjDojM3plJMgFJhjgdbGkBTPwQoBAK%2FGBhFfyhxm0fwONto1XtgnC%2FQwqpOFyAY6pgF62102hdVde9vlo%2BFsCbp8i95xukRQjbB%2FHQuOToJOFz3omyiAtJ45dMwlWeYuzIOKvJMs2PI8q8U4ooTPLUz%2F49%2FmYnWBnwYNxgPPn91hQ3FK1%2Frc0ON4hvQd%2Fw4DPJV2CJu3AerDl6k9LOUNoaFHHVzbg0vYWggTpEJeHvtRHnvuOdd13OFWwSXrNZ0E4y3LYOuu5kyU%2FP%2FDDEOnjLTIEAmmpYCC&X-Amz-Signature=4d84ea09dabcd05cfee132d3471e7deef7a7c56f89fd5ed17907beb87c52bbc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
