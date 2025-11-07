---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667G3LOTV4%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdSNp6mMGKs%2FgKue7xUi02cizSD7Ftg%2FszGEzQXhAM7AIgTRI4Ueu0FiPm5jUzf8d9fJq5G0lK%2BqhwKyYUrAuhCc4qiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIgr1R1%2FFYQtlc3x9CrcAx3xeObIzE9E4e906bkfJD2fAnw70Wq7bqel9owwXsFP5%2FUGZ9eEkfwWQ3cKdFHOsRjiNL4dOvOx8Jg5Ci5Pn5DiHmnTuroC%2FrGnqguz1USt565ctbRBxIZ4nYI%2FMVAsXS3lpHh2v9EZKPT0aer%2BPlvYznSSubFxvNF5L7aZHH%2BQgxDlOv8vsPAT946MTtgfk0rjxW0GGIxOVj%2BlfNk2USbT6rctZ8OTuLLyuFA6h0CfnTSefSsSNX8lJ%2BFngqPTKI17VNFeQP4t6H9igKMw%2F%2B2RgonVrErElz9IDQSmD%2F2BhFdI8WTNeLtSs9LQ4zeRQunZLUqMK%2BpxAo%2FlAjRNz%2Bd9IctyyQYS6Tkl9kMS1FOq5ICZ%2FBXLjLZrEgldKRPqBbcExNDno%2B6KDCvmYyBBdPxBUyhOqbz6XqQ0U2dQGt88BmCDjRy4as6ZpSNqZrBFyz9spTDCyVyRhYjgqUgKrTO8R%2B08Ky9NO1JxX09zIosTz2a7JxJ%2FfxP7HdxgCQmDfU1cUkkhdPIziXKgSsPTiMc%2BKJ%2F5YvTc3BBSNlQywuGVfgRU6H9Qkn0qQHHma3j4yepyGGp%2BLpQ89lpP9i%2FDRXVtonLrGqnsEZNzY5Mk96jXcL4yJpu1OZf%2FH9MeMKvguMgGOqUBqSA9G3qwcj3yUpH%2B5eNXQRiEhmkvj3dntBUaP7cFAeYwLwnG2gPWJn2DknqkpsHP7vidRvlPpt3CcYYGnip1uuM3GCpNmjfHTmfksRPzQ0HgbHA1Xd5eZqZdhEI%2BrcfWxQHX3y%2BR33L1XK%2FU5T3ocvGmJLbrFiPt4oiIhiV4qjj5B2VMDqS6R8Cc3cYpMMtKraCswZiTVtOtSF72qQoivJyhh3kO&X-Amz-Signature=72f3a0a07a76a93277be433d8239756923a56c128614bfb604d610d37c50243c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
