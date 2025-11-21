---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQVZZOE7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJGMEQCIF2CbcGO9az%2FRUREwmdwFayu0mNt8l1ZjZLChKZqlDflAiAUGBGrVFIEQwt7FXjJtzCdO8453Wmd3zUtZtcZ6kh88Cr%2FAwgJEAAaDDYzNzQyMzE4MzgwNSIMKF5ZFQs7tPKFpO9DKtwD%2FLclcr8zkql5ALCv6%2BKyoadAHrQCCS3OrgcUwB0NhnPSebL0DhTt0yND0dZOECXmaV4V40ZPvgu7MCxfYivzIa1va3wJp%2BjG7C%2FS5to4b5au4Q1BeQLX7g2BxCVTYCQ4hBbV2K4L3DsN6bbc7QTdGkAxJfXJESgE7vexf72VqB2I84ODowrN9btgND5QEVtGVa1QPzDiKPSENsSw96rbNzsArW6riVJSTywjaiuMC8kcy9BvqfVGPgOWXQTM6Er2LsTrzMammgN%2FjX%2B9fiEk1JXIMSn5T426tfiDS47z36tpMbyL3DYkIDkKpPVbymiGg34pFfoC1z%2BTcpiFeYDbYFhKe8mzU7x9vLrUWzsiaysr2NneIe8whhKKM%2BFukcv3nZOGHZ%2BQ0hZEVQTMVJWq7RDZNcEQP0iCc6iXanB%2BTIOjOTCVcer598dJmviBsXeGNXE4NMbthMgaJx9t1%2FpMYmMVjjU4q%2FgxxeQ1CyobF1MrNiVNIqDRD5EYa5B%2FKVZQDINtrqY1N8hiwUzN34%2BPx1CwKTkUKJHGZyTY3nd9pMx69kRUfRroJVCBj4vtodshPBSXSVazlo7eWNKKYsdpZ2ge%2FBfcIZVPK2mveHNE6OF5L3vIioYsm8fMw3wwi76AyQY6pgEYCWGpvpgnVHXc1cInGgKaFd%2BXzVf8icr5xse4QWEO4XKDkMDrI%2BGThkv2vAHOmHCVxbz%2BIo4R0tSL2Td5LL%2Bc9sDxYgSTcIvHMTkHntoa5nlu5HkoY6Y9DkeBQK4Igge%2FmExUAWgRZqHK8yfBODSx7gvTNzewmTwlO6G4sPww%2BOGs05I%2BHTuVzogCUztw30qKG%2FZuV5KLP%2BGcwHwBVwOAnuFI1VW%2B&X-Amz-Signature=d375f69a374276c41eef7c97647b3c9d7589cf36f107083a4fd168118e118473&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
