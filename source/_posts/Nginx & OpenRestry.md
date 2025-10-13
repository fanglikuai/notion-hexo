---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYBM3BZA%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDpJW%2FEnss0iZ6%2FzKAdCV2Sc3AMZ3MxhAbD%2BZh26liMBAiBOtchsq9rxZMiM0mqaw%2BQ37%2BZ5NLgj0d5zR1dSdIVQYir%2FAwhIEAAaDDYzNzQyMzE4MzgwNSIMzZrge9MWKopTeua3KtwDIpiFhjtfLtt5GqwOjPqkZQTFg7qyxwIeKte%2FM1OP6%2BLRBJzPrGYyzFB8vCS1bF%2BLCCOHO47SYeIGE771NGaEEpNgiIQjZ%2BoAtBqBz%2FCYjoZT5YdJakcxyEUiwOmjhLbggop4opX9wI0momYnVpBgrIgNagsnhtxmJd4aus%2Fa26A8VonMPHm4ZYJ%2BCpx%2Fx%2BKwuj6a1sHmwl%2FNESfrzb6GIq3%2FvsqlvEWQuACPLsUhmuUmhq2ivU7HtbuI4KPodPVzb0%2FtSTwuiT2BfEvaCbA2vLt5R844zZsILRHcG88G89AA4DeHYmmpHgp3xzy06MZTyNKiowxSTVPLDkGS1px0XBtsp5notl2LCl3qIzs5nIZfd1%2FRECU0oPr2DQU75V5yqUXy3%2B3UTSsPo8cHGapAW%2B%2FX8t22%2FX%2FT7kBRyqgnZaZZAk8afms7OizDx3GLTXnRJxTNVOOqa7EJbM1RbQfrTA2s0%2BXLkVCOZxro3ontTHz10WH5lOA39Xy%2BJANUyPbpXwlWbTHzCc2jZtWAg8dzzpdAGpk%2FWib460ZrisFEO3iiJ3pqvFPnm5Z30wjDuIigSzbG0NTiAJj1Bj4dP4pTAvBTuOy16yd5k%2BpyxASpWAekItp%2B8iIPle19t2owtbG0xwY6pgH0vMjpW591ColU%2BqeVKn0oKUoW7mXnghGiSL%2BxB0FBBD%2BDtb3uFPPS9QwdlwG7eJ5LRI7RvhtEioGyMba%2FkTgwBuTudXbgMj2jQ%2Flye9%2BYXf9EM5WrfGEMsm1Q0KOrC5gdmolPOzQ2ZkqiztT1l8QF49LoDdSKOi%2BgJ9Sg2UlSKHS8MOS3aHfAwT3Jw1dU6MfYLvvEAPAgUO0pgCiN6HcF8JoaQr%2B0&X-Amz-Signature=bde9e2bd9f004bdc208f94d2aa5a95f40cf632086d21847901c459ff4518dd16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
