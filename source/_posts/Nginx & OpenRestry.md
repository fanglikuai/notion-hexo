---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4FGLX63%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIDyQ%2BOJbYWAQoOu5a9mrSu9111cGefZDiuV4p3t4AHDgAiA7G8k6pbhFcfvjDgBdM8Gcztwhc9cGSZ33jZnDvjFEnSr%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMCeYvbbSHd%2FbixWfUKtwDb9EwimqbuoBc2vS1WcCBQDfCD7kWgx%2By3xZThnBjsmT59a2YU0v9yo1NYik0VG6C9sz8c%2BwTtlLXWcpyJv91cCtK31iZbjXracUJUnbLYg57UeLRqA%2BxbSXBigTO7QZhG8wcdq2CQ2ca889U1AECreyYW9fYq5Dl5uhzxP6JKtLuxgv5yzNvmbeipabDMnQEeMUuukQMJ4%2BKtlXaXy5%2FKGp0OQWBtV7dRbtfsMCIFFpPS8yF1aXETWH8mFYDPuNbMSRgOdTO%2F6R%2FLz4QaF7hZJ3vbezaXXFgjYTAfGqZI1wu6X2BnQ0TXBruotQ3pvBXbHNsoE7VwUqNGQW9WX0ebq%2FvktJyS9N8TgX%2FaA414fC35aJ96xrjFLFhQdMK9nsmeCrTA5UtbVSvqUm4rBhyCkMYGcg%2FbV3jiBdoxbjSLzPLWKzjLdCxwRETBGuMqYVqnqvgF2PQiWlzdNhaTA4kYSn8PJn67OexrFH1zr8No%2FwjvBSi0P43D1je7Yu0NWopYQn9FORlSxSjwfGJZ8C4TU56W4rkbdxEq5ELYx2lm0JsJMqi3%2FdFuzvdGhSphQZJOgxDOUFv75JTDELcZvw2lCK3YhEEmJuC0ytJX%2FphXLM4TC48m%2BxI0q6eGzUwzYCIyQY6pgEPUhjLtK1A2e%2BCj40HWelIbf1xz8l4fjkjBnBHeeKLenJLOlpAxLd0VxuZ3AYkEVvEmGnF3WFhr0JQ1f9ZGMLxuBBr8FRjGswSLS%2F72XJHNhLCCXFX0gKvqMbp3dw5obJREsT2KrNy7vppiyOU2IJMolRP48GUgQtkozUAVXXuP1bqTsyz%2Fk2AUOpCPEF7k9KYI%2BD0k8MFlHGIjLk46EVE%2BWzxe1cu&X-Amz-Signature=f159b7e5959a4f90e88f75b7f86f14a9b3965b493af9f5d1cf57a1b72231a6ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
