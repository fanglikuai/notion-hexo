---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666J4HTG7G%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCIQCgw2giCIrRCPNQsh6%2FwSKewqSd5ATutvJD82uuv9KZFQIgA5OD%2BFCXsUuH7fbFGU%2B90SDK50bLEGge2K2HRY3YwGcq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDCnLFEv%2F6NxtspWddircA0Pt5eP5d5itub3XlNSA5sDZrnLjNrxnc2c10D66hY5k98Y3Gi%2FTXST%2BKgF2KealHMQpgnnXeqSv8bPJJzT4wdoBPhGFXB7c9%2Fdg763XEoS8ltTnLu3GjIRPXOp3UzDlkZ0cLt5YWHL1M0x7kz6gdftKr4lmg7P8ByKRk6V9RURGgTs1jSQxMR%2Frs7Kp1EOyEloLnWRsro%2FZKCo9qLA24emvqhtunufr4sqDgahT2fnpkDr6tMXSBwvSWoCXLTBG3QqbS5i7FPSbtvm%2FvFWlSo5u8y5Md2ZSRj9aJtYLgvU12rGsKiJwh7MlUu8tKM9FMitghIPjdnvSWESU3UB3%2FI6YTZxSSnlOqC6avbK6gVa5lAAV8JF3ht2w0Gl6EBv1b08B0MmFwwhg%2FfLfvBb%2F8blsvPqiFW3zaFXgJMt3%2FCsjDsa6qIk4HabE1hRvoN%2B79I1FbphKsp35mPkylos%2BDP4K4c4cj0qlhJqq%2BW%2FluyXlShziIE1FVWlUtW2RViTSpGHGuQ3RxJD%2BKFMpLbXP2OGpZEevrtUxw5B40h0Q1WEbkaQqUPGvmIG2a%2BnhmWxGy6oOeKvdAPm6Bddv%2Fx9wLuoUJLyL5R702agmVMnQ58aByME%2FrqVnACh%2FfGSVMP6mg8kGOqUBi7OjqEEYQOpHGstKA7vEIHfcFelbqgHpTEY8yPKj23pEa20ZsaQ32C%2FR3QzlywuaYN9lQhgHxvjX7vVBBaKPv%2FFAVJZNJuskVxWIAsyUFUQa7xBR3w3v6k0xmQyan2%2Bh5VWJksmUlxpC25Gqimkz4NoKnecH3aZB0Mc8Km%2BZjHQpsykjYZP0mx9sM3NAj6owB9ubOY2gdbNQ%2Fc4g4M5kW8EshntT&X-Amz-Signature=460560f3c4aa4c90530a4710125da3e2a0ef649ef3068f83b6e6b8284f307efc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
