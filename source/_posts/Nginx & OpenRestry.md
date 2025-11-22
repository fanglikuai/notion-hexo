---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2HOQBTT%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCoVKojA0Oe9DAFX42X%2FS9uRs1A0QsjcyZ5kO43b9PPZgIgVC11VNitWbRi4QTZ03tyitghxRJJ0ZyxlKvSh8FDc3gq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDATBwefinMClWa5r%2FyrcA%2FAyVGOyr7VI38JjLdP8J%2BeaNkjhPPjHgsVE0TjrOPvGoIDRaTVRt6KSRFwajkqjcsL7Y7qBwSnQYgo%2Bsp3OLM0DlLjYKzbPU6BQyyDBrDi3E8LzLl2tLb1gJrClh2o4bAvMIMTq0R10UtZlNr0rP0KOEVFVHPkW3TlTtoDz5NMZZ%2BrvbyPoynXLtVR8nXeTQTYGp5KnlO5Uyt98bz0oAwi5lI8LFxsKMopfP10hIjf7tN0ZSaHhbFnBCZgC1QNYTZBA4WyrL%2Bm0GyLvhZZRqSt3pv%2FWRhG7a8zpBYbv9ELjF1Xn0zIcumHpD8oG0RqnUSWOxv4RKS%2FBn2lx0xjJr%2FbbJD5QbMNbApddUlzzKp3Dy5D0avujXqjRbb3NsJyRXepQaIWd8OtdIrzpXziR2T9YYmQpPD8QIFJZqb85cu9ntPFMo9RoR5LShxu6gulzuq84bVmrdEo9h7mhXiVhlkBa9N3l5lcVitybNoM%2BQQpVpxyD9DNb6c1WA54gw9ssTbVgq0TRSeqpBav%2BGCTD32mDTIVov9K%2B2DFlKpYnULTuWIux4HaIsbH3hYlPyB81VVDc11dHVTuvzjaevnalYgJfFJ6efK829VG41ZVQvcR%2FPbGkJTSTgh%2Bdk%2BsIMP6ghskGOqUBbNmI8Ud%2F9nIK7ohMKaKZgOSzV84J4ARNEztyYj7U8GAxpMcG9C0LJWnSuZ8UXCbBRvPHdKmR71o54RM7D%2FydCQruvUap4TpQ5dlhujcraui%2F5NYQ6qwHb1IfhZsQtkanI5uNdC1tTEJTy9A8eTnZn8lPhmPB1ZjW8M0EM9UnC8QbA7RHPfoYUK%2BaPwyylz3ocDUIbSvnIj4KdGSNkQt%2B6RSaWoPi&X-Amz-Signature=5c982f1073471061b90e954c0098050e1d818804d4a6744c4b1f570df4cc0611&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
