---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAJQVQKF%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJGMEQCIB3RspOEyS%2BbQvou9OyBdPxvyDQBNRb1pTJVP0p0WVYaAiADHb0Q%2F0Nlv11S9Vt8d5rOpNt9c3OCl2SqSzJNCTPwgSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJLr2IJ2xzFHzOflbKtwDg5p%2Bt2voxJqnmVa6X5VAU1JHRuUCOwED2H%2FOWEMDRKr%2FGCQUPCip133ii4Vku10stO7d8TB6bvfneDvdba%2BuxKvxGCjc5soV6tSoWDnxdJ7hmHQWnQCAFhiakW5rIeeIi2nVs9rFJP3ULtUdjt%2FHLoOblgkAd0ZiajUH6wVDBpJKBa9MZuqQ5jQdGviDS7hkzxnRM5tFTCyL7GDuI9wxvo65UuQW6689SGsCXv4GIzxASJYIpiaAXU%2FhI2DHXoCjE13iaWUGCmlU04%2Fih186yud5Ii%2BGNhLsePeuGtrcbnFm%2Bo4XlOPIu80U1N0BRBLpxYVPvZnnFV%2B%2Bn5fxAik3o9aP0ObB3FAe67bbD%2BptB4F2ckSQhpDVtshmRnZ6okNIqIX%2FHDx6uwfnnY4828j%2FVakhZIOvQJz9tfYw99ER5IACTwHViLoZrc4QcLmoHl000n3VSOP3tzGQ2YJWiI4%2BZOB%2FFXh4Th9P6kPnqAQgNcAYZhUa1IFFUdTjXhL1YatCbcPk005SJQq8TGaUkwazEiTRaQ4xnEj6W8z9aR%2F0nvFVCm4PQ1CIhE8lq4ztfF7SctdYihz0%2BFcmzhdWY34WnvQ7xkawhqZb00LB95xrMWtWqXG7XxW9D4FCiGIwg7SNyAY6pgGTRreL4q99AMPaqJA8dnHjjcCnodaBTpgcBSGtKXRbDFt3u1EVW%2B30Ta76XVGG5wX2GDWMdHcROOwyok4tl22SM5DwrYpHMyKDZ7jc9npD3IHsRmcCd5H2k%2BJ%2FqvfF3ElUWUmcDaqhUwU7%2F7%2FryZJ4zbQE5dmVwYWulPgANSFCNowTxjLeSEg8aXO%2FDBN8XNvdJweEqVkLYBX4xGajMHPVYkBAxPNu&X-Amz-Signature=a68ed2232b82c8d674e95dec6db9e02212ff564849c1848bfb5ca07597ec3b67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
