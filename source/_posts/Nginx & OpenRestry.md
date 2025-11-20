---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SDCWQAGD%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T010057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIF9B%2FDW1d4wGlX9UgPGf7Itj3RyvamRn4raL8zeB1KWzAiEA1D%2BbYFgSQ%2B%2FllC49pEYwFWBrgKAJEJE4j%2F9TbQM5RzEqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOBZvBgQf7r%2Bx1fp1CrcA9FV6lZyDR33tXSnTYXnevIgziVzypV0e1TX3p4XPI67GVdoN6bVU4DsgDxUNSdw2G2kcwXBNJ32JPHuUC5BtAzFAlgQoplTF6%2FAtjksX4%2FJWOs3Qr3nqbbZNUX9aL7l7ScDWBXvmP29P9PS0qSXUo%2BWl39sMDtMGvnGofh0i4c9Jm6d1P%2FU6HanTdlR6kDezLKGH3DyNpbihSSWbJQT3CQfHPeKGc9kH7DYPcsdAb3W8Wamz28Ek1prnQ7oRkZJz5bmDh6fNvuWvrtM01XOGGHutFPHs%2FC%2FD5lmX8VFlWkvPf%2BnNBk8m3RxC3wYe2hVMQjOQsF42XpQbQOH%2FIkoP%2FQNWTA2lcW6WCXYGZeS%2BQ5uz822%2F%2BregprzEAWCLJKnMdplB4dXIJtE3krjBuAwLr8Vy3PKP9QechDb6qdpJX2saTDvAf6TOw3qkvpvfO0KRLbedNhGx4i9flfn%2FHRm8aDbsP25DpzCYMB5g9Rx1%2FhtnsxJJNkT4bBXC4TiJKhB0WTO%2BugkD8pNGb3IOtpfJXpEEcQvk6cY7%2FE5p7vWFk7FZHYTtumLhOw2dUAFnfMp1bPr0d2b8xLls%2FvmEm4bqtvKeF3py%2BcrpVW8K%2BYTrlP4VgbTdnIUVV38c3zzMJy4%2BcgGOqUBHRfooC04cHyHSoeJZAjuFlyxD9r70vSyvBDX2MKEq2%2Bqr00KZeS25HlWZgnTv4UEJgNeIZ5XW1OAFtuMPwpVuM4XQuPEFaMN%2FnSmmNPusBfeSZRj1H8dDwuhZqfm6mku8llaE1HkUEFC3V7AwMxas0%2BzPdIkbwH53AmN73g6uvunc3ZuVHh8KeKYmsob6lA9S4j935WA8egj%2BVqq%2FyKlBApdEb2R&X-Amz-Signature=44b68eee223941f7902e77be7b2b003b3314f6c1b21baa2578d541fd552abd55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
