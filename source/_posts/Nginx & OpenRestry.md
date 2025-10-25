---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJTWVJTW%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T030059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvXaPglCbCqrsmUvlHqe7uZVQ4B54bsBj6aHVqIrkEhgIgd4G2R1JzOFU0NEMx1R2miCJ1QoeZ3iRiBJ344gR42sQq%2FwMIbBAAGgw2Mzc0MjMxODM4MDUiDHIMq0ZxD1QU0o8MyircA4T5B30cQ4u8bp8FnLAvlsx6Y8OdY5evLGh4jlxRVlelDSidWCf3HFWGVw60P3HejrVhgRUtVWJ39RTK2vxpBIHqgppV%2BrLtmd58zhBUnBwFoNt%2BPvZ0xHMk8RNwnj4Q6CyI7zzw9LXnBl94HgaaP11eddBlkM2gTvc%2Bf9%2BTBnCJs2CfzRqapyMmh3pPe0U2nUTxnLrB4vxghyGRQbwjSqa6ZZMltpRpfgxd1Y5y5xIc7sr4CKTWHaT9KUc88gaDVnDVDfH8G1LzM4xVq7wS2NPiGsQwZzAT0O7EdK7Ngm%2F0Y4Ecmk%2BBkUElDYkfp8Hl7Y%2F7J%2BepxtLh9BpilnVixya1RTVFRL%2BjK0Gt7EKmnsOKZrNhQ2nfeDcOJ1vwKksfNuFpkDa88KX1img7jTZcoV4GWSnQHcRlbpmCbwPE7paVvRiubu7JI5mOQNFsqp3gXwLeWRd8C5N35gWdjNmrzfmbewlkAhLeStYy3qR278EdtJ4V6vl%2BjpEIociM9KRICxMo0%2Fd%2Bs6IN8CLs4Sado%2Fis%2FqHR8POOVDv1eh9yfoybTo004B%2FGXyciU%2B4r0oT4w2niNBNx48OTH3%2BZfCG8qnNX34Asbe%2B1HsDoxrG4F%2ByKWd86hFbr4h29hLANMIvu8McGOqUB8LL8E29b7yJaQ33m12f221Dm5BasJhmNIB3zrc2AGQIRh8FTbGo9kZYX0yfLC4x90Qdc0AXIhTzLUrGBs8%2BA59BHM3%2BwSL0uVTs1sTSNKPuCWmj3YHberI%2FJN3ycXLUxeJcBPu%2BOQ2BLXn3ZMf1hzeUDfh9dPcvN%2BtuzA7EvqwuLOiTexNj1EZ%2BT%2B4MnGaiNotWXr%2FSgCAj1onuIJHiZWewB0fss&X-Amz-Signature=09437b7e37168052df62550c41a55ce4c53e7fd526b0659e7ffcbe3466b7a497&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
