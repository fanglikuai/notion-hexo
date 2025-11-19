---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THKWRGYA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIHkhI6CSXOxc0e2coUNbwOvLIxNDTYvjyoY6CknLXqURAiADYlH9IZTDw3eHM%2FkPbVI4B0AFGmqKU6AFc6ECabN7KSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMp%2FAwN5k5p53qeZgEKtwDsFr168epwgipKpQivQknWAsTdt0kJ99rI7DamLYiwInmwH5%2F3x1vsBqena4Nl0gb6km%2FX2jHXCQphLe8YH4bniFfO4sQbiHEW7dpuWFmDdFQZsHydDx%2BdfayNsT0w5QxRsMHL55OpgbVx385JotSBF548Y3Ciglz0qEayUNnP8ImBmAGXrKobdQbwst%2FnFMGR0yqxFI0ojBYIStiWYiNHCf9HEq5Pev07jeUHWez9nqqcbnToPAkupYK93VWwhfIaraKvXQ5xVUuMcTddP9fYBXRj87MFVHcogEWRm9pl4pkaPwAqVaIn8YFymTvs3r%2FF9AFtOC2IqcId%2F5xjsWHknCBW1YQ%2B5XpP1FNGNEO%2BWdthbTf6AJvyfDQdSS1Hp0EakaW1jklOn0I76C%2BgaewgYF1AH5rfNCHLunWdDRzuFpb7N2gMo4FqmWSf0ngcCgGLS0tvRhqCRGtatqvYIt26UQIxtEGbF38nYCL9LVZHPp4%2FvZ3rkA4N3SX8grnL%2FrrMAK7Ouoh2AacOJJtClX9U03pmUU2nfz8C0qctA54zPaQr9%2BL99ohSUr7Bm2Jewqyll1MCM3eU8b5IpSXncl9lrJFcBKkjquXtuMt1R%2Bai6LynruuS9s0lur8U%2F0widP2yAY6pgF1vSCvbcS1v1bLYDo0cDAVQXi%2Ff0p%2BDl2JaKoOWU2IEGxQ76vCCVI9IOWOPP28EFDQmnI34T2dkLbq%2FMP0XT3JRKSYrKzSpqjAEJKNHhV3DTr31xBqtTDtEsKTXDbN80XZ2i0tvSBsuAej2OEUCmgTetO1zqHjvCI7JVSM%2FgcpMUZ%2FMxxVEf316e3jOm1MmpuMFqD9HZL5lhD56tbuWncOliAPAaFW&X-Amz-Signature=1d43c5516be2150bed84ef7d736f475e44d8daa511aa8eef1ed3b8f25d492961&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
