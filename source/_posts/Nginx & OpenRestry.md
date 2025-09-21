---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRXUVK57%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICe4qlfILdN9px4msIaUiJCdW8Kte1yZ7EG3p9sy2k8jAiBzjYIDAefD%2BLZ0Te%2BCqaXniIuDOmbHiRiYLfAR0dk%2F7CqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQ6sNvRqTURCPofeAKtwDks%2BbkVQ29pYn9QRwg0bgsnNSmyL4tDPqPbP8dDjAHu0Y8Oi4L1akaDz%2BRYN%2B8uvPQ3lV7e7O8nN02KpRSrNMSuo89%2BUevmTxZJj6llvnKZhckZF26hNnKLgLGm8%2BfsK5blEWHX5BRssi%2BQNZM5nxslv2J3D%2BepvxEBd8hnIdGoqB9NDo%2BxvBz0YRjWHncaeUq0oWcrrK3VJfHm8uUgbIRAtReOT9QAkaLt4qjM7pV4oDbFX2VIGyjic8DLjoZtiwZBTZysdWrGa2QmG47SQgUsLh19sfxz1hW39hIARW%2BjQK91IKMIO4%2F%2FqhkcmxXkSwS4yjO5TCTqQyo40WhHKyDPDGPBDzrb59KLttpbiKUyBqaKm9i19V7qUtNHTHtjiiQkSOoanFw2VOCB46w4Y6wA5b%2BUN8p%2BJUDh8p23JjetrNgZY2dE%2FEjsEszVZzSdwAxtdNc5cEhYunsJWPalv6tg9UC4NG6gG%2FP6y1twff213xhJho03gbdUMSgX%2BTSkGCF3A2j7pMl1UfyZ28ZLXyQT9rZsardZ8fUVCBTWu2vphjCmDHamMVlHzyl19izLsyZwgYcYP%2Fkv12ESnGKnDwEDYTXbCIZsG3KvbtsxZ5Qs24FObFxyrh6l5Ab5wwif%2B9xgY6pgFnqd%2BwGz5Ip%2BWEXUJbmJI0oFzFEKGur8wq8uZw6bbg3GYGak6Tuuez6KJRfcbGv64ZqVzR2c9ugBM8mlGdF1zkc%2BrDV%2Fp9z%2BqiVDRcc0Gd8NZljcuBXaZpfzypeie7aCqFUd4Ohn1a8UJ606%2BOpOd811Mcp1mFCjD6UjpuZ0FCDbsqJo3SprqqnQbKbsINf5EnXm8tCs9ZWoGSvACcy6WQ3YKRvP%2B2&X-Amz-Signature=7522e4b215b29b78fbec55e6f1a45acf4879a4214429a26fab42ce668fa16384&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
