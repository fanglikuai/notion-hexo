---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDERM4RD%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCD8sSP9AmoeY2funDgBBQt2Rx1q73A3g%2FAdMnen5aLMQIgA%2BL3RuqmDQPBponygLVTdqLWr5WxXHurbFiCrHS7eWYqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJpjBZWK0SBDc3eCmyrcA90gTOfclXoznIiLTBmKxRA0QIzwEYmZtTNItQTPInRYJdeXcir4g0mKmNoM9Z%2ByrFLd7B7zVji1xDXuLXdpgWhguU52DBzCCToZJyUnJbQeIbSb%2F5rsfdGNWucCgpKJkCJIs8W3HK9t8bsUyqSQmjDWbq9eR%2F6r4oaYdOrL4KQMvtb%2B9AXTOihZegkdSS8wIb%2FSb6TkqpLo6ELG4asasbpzbybta%2FukKE%2FvuEQmYrUc7wKBUhGFJVJQQ80g%2BNhSfliVJdsdjt1zMvF8T%2FIycWPR3YXDTSLaYbc91%2BoMu2Gh0NnpCT3RyKW5w8UWJuamy5myl2c5zTv2KAviuH%2BGyhMlZWskdlh65usz40dA7mT1WQpHQcL4vzFycVQCap6le0hCMCpEnyOocW3TkbygYTA%2BtSHsAdp6%2FzS7M9Vt88A0bxjZ6syUCXFRpQR3FXVRzta7CmF5LnqLjWWzPIUOI4liKX2rkhrmci3xjlvpEfMcm16yptZWSW6vdXlEaoIZexqo2oHEjyFOII0y3kzQeeMdjlNuRVXl4dy4aKRv037LUeBquE8X2H4BNpPNh1VBIDadnzmhmj1rhWKSfLxBFHQ1PZzhz%2B5YmgY49MXOirkIor0FF%2FhbHla6nk1eMOKJ7cYGOqUBchdPy8HbAqVaFfarYQpCaOt5L8PrVo4yQz%2BDTc8ofsHFxg5j%2BLN3j6TrDb4uEagS87zYukKAq2VpM%2FN456J8ZpHnkTdBf2nHIWfYMhVOJDhL5BH%2FmJCSc3lDrGXTMe6tk8XAtMIMDN09ARA9MSWv%2F7xzvKMh2m4tyxr2Y8%2BcX7D%2FOvijwfnQP6%2FNdeTWrNYFkvgZr1VXqmzGY%2B%2FRTBCgrb%2BZrM5u&X-Amz-Signature=2c82669b28d09457ec7e0850c1d19c169b8e64ad8290ddf1a7af5210dca669b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
