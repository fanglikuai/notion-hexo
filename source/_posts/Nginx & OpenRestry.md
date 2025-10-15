---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNXDKPJI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T200042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFzHRcAvAIOJZEaBG%2F%2BAu2H0BmKDF19GESDgpaiJyiY6AiEAwZ8cmM7Erz9kp64T1376WkD9y3NgR2q25teL01sRdvIq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDEgMw4WbstlzoYjqTCrcA0dcdcG91BV8p1KgEsEApbiE96wlgheq0b9zjLCZ%2FdaUyffDaQuoihmnzO5Dz%2BRTHvHvkypffqkrW%2BDOTV5sTjmKBZhsqZZblcG4CA%2BjYTcmOBnFY4vJUZRqdU5wA7BYAmL3rNd125yOQ%2BuTGUOL%2FSaA%2Bsebb9RiwisUf32yd9jfUCqn3O%2Fv5BwC6CGGlit9UwJpklURUjQaT9dkGlyqBNv3bLBFXp7Dsmk6%2B3r3ImzNaiJvaZu4ki7DLj3k6xjCzF4EJH1FNunJToiPaInHOQoMdSXKXqx3689a%2BY40xODL8SrQU%2Frmj8rumpupWp%2B25sqwdCzaTx7zU8SVlwmHgZWcdqzQ57m8CeEC1cLpHNfwFfkPkjnUSLHT%2B3Y5acSCA7Y11NrH7uiQygemKjDx%2FMqtVzDOyNb3e4aC8ugM01DQlTCMEpoXC1FEUBmRc3PmSvVyDM5RtIoH2s7f7hWQjP%2BTM4YXopSk6fHoLxxtaMrr0iJrxviyybiQFJ6gQvxRX%2FlBrmqXnpAE%2FVmVISrqpmva8GzB837uZohqMg2Ijaz7GC1BPMMq0Wgki2bwx2wL8BlgjEDIaX1jjfz3HGROjASBGQbnV5%2FaPTKnnao2%2BW0Km%2FBE%2F0%2BkJPmiVUhfMOfsv8cGOqUBP0lp61GNEYyXcYl%2BYR2gw4OvptaqhZ75eAqlRiWFJQvbi10oF2OdOGc5fQekADl%2FAps495OSds3u1ZcjjTFxQFGdxvSCqm93iwoCeBFLBbB4aXf%2FF6UEBo%2FhL%2F9XzwSlsSB%2FCndHV0ZBJVv9y1SuVydOmq0taY6dGAHNSgvQIGQP%2FpLcVecJIY3lmYuIBgDdm9z%2BC53tKcxoiu2JYt5qq%2FrZBUib&X-Amz-Signature=8a6c1fb482afd96903a85f7fcb211d82be60d175a6b4f4b6e923d185e103d99e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
