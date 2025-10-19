---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DVWLUJJ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIFMwbVJzZ2mV4hM90SglyQre6CUxlhpMlkUvEo%2B7gH9MAiAN8dF%2Bv6UwRyjVnoGeL4593I2AYJY7Ksi%2BBvuyOwE%2BgyqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWTf4vfLNUNPqSmdkKtwDFgjG2iLPi%2Bj%2FgX2ltDYUkTE%2BCWRjmlg46nfVmQtOQSJOtKSKdJTTYxU883xCD%2Bg%2BaSJO4jBjEhFbNMqkqirZ4qRrAiZRWLNgi6Em0IJvsoQ%2BnsEclndARJOeEvJXcqLRTwEPrR%2B2VDCmRjJLg5%2FDVz58oub2YN9VOdjstmVzoVRriKt9K8EY21NtLKzHCKfd7%2Bk%2FX1LwVdXnCWKi7jzyer3DWEC%2BMUjZomtCtN9NGTqIhEOg1kdpKZ1nS5X9opESFkNwOscchOOelVL0i2YHyeeRQROT5yz%2FjnS4PhNhCcPPfSSuCZS3lIfXmvkaWoXlVQDfaEzKS6Fn47xdwhUdECJvCvDaAd4a%2BR2h63HXAjYJPPMzRdCXDIlMS2AGP7tJYGeveyeXbVaNONAhMqLvR12RQPRwxf7NZWx7FfrhuRPO45IcCSBPKLeaCISHUU4RTbmoQeaS0d%2BPYzFk0UQ2eslQA1hqr9bxXG11SwqH3dwCKM%2FLVetq02q5suL2WctaQFSvMw9Iloj4%2BisIb18BjhJjnLiXe%2BaW7oUmYcN3HP1lOgE8dl99P6uBsa6qHq4gSqDcdgYHBOy5tm8FnuW5Yl9AegdNeTwH51nLaH%2FvAsmYE0Pj%2BPFpjZef844wr4XSxwY6pgH3B3cZPJKNDMl5qM2xQ%2FBPzHS3RpYR%2F7BjaY80fNmA%2B2Xg7KO0jLGu%2FnuH7gmRHusIPc4nf8q1Jcxc3IcKezVHGAn3JgCbkFo2gyhDa%2Fh8P46QlxDso%2FPEazuhNLAtvxAt%2BV1fRKiF2b1aMhZZedC1FgukqKO54O7cOWEabCqwQHhWmXyKAJwL7pdxSYXwvba474ZELe6uw0v7%2BOu4bpKSr5qUbjQk&X-Amz-Signature=6f0b22885fc34a26048b22e6d4b7d3072299dc72ff1aac0d01d4378f00f028b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
