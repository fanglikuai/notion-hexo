---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46637LCW6XU%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T080103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCY0zLbp2GH2xBz6wmrUL5f9lYLVR8JnjEswe0HBNyongIgbzpxNjGGQp6DB2p9%2FWFSlkUH62Vcbz9tw7rbmUSIBpMq%2FwMIQBAAGgw2Mzc0MjMxODM4MDUiDAAwn8hUaRmRZyfONyrcA54DiLLoeD0GufOP8M%2F8moEz%2Bhi5uTaMJZTxLatx19Vnyo%2BlmtcyWB%2BrjHVaCnJ06dqexdbRwxwdOG5MAlkdobz9fsyGg8Y1zHa52qxFpDosVYe%2BLpAhGQR9R91A7Mati6%2F8aXz9ie9BxZKcrGM2n4B1v%2Bm4ODLWlT4kkU1IVU0YCKwPtUYIfu59I%2BxFfFZr6AIO7vFQ%2FN1OKJvV1biZpa2yylvGSw1CLCQDtvNgj7NQn0Rvq7xg%2BjrqjlDNGKrS6Pngxp6cHAcps48KlJBU0vCxMGso301OQ4EbXTT8AFih%2FV2Un04iOMGTceDU19PyK%2FNI%2B1L5fws%2BaCtHSxaF79owQYs1OwlmypfSBSgsTlfQBKa3p9dhqYgXe4CSMzZtyk4g%2BEBCx1f6MVh4NPSlRTwCkKX5OtTEdBg79OPF4%2Bd6UeBW0BtlrkNtfueK4cn%2FhOXxTBafJP9p2fxw9Lpo6EpvmrvInLtxsU2WsiJF1%2BHIfs8RktiENwmcVM9%2FQdWzhYHDL%2BNG%2FyqVNR8cRXCCwIQhexa8GbVfSeiEg9vC%2B3MWSQNs74GZ6Bbj0DFvY81Yir9y%2F0rPsnv0Tq0Igfp0pxmzZXfPNBPUOVD%2BU3FTF04x6SdGK7XTzFJ4nVm4MMOr58cGOqUBugxNovA6DX6T8fHm5Lf2dnfF2oB6sKNR9SjlTig%2FWjc49ndsFcGrvwdfDyT6lnXsj5o4q6MhDeBv00CfuC73urBIIPld9Qvwrcm5A45F3P6z9VFEUA2nXvOyad8BSTWPsBAPYdcYKPzHE9H62iv6nJeAeu3MBAzoOEPzQkKgkQYUB2Pz2uOQXyo7GFUSyULphqyTWfLIowuUElpteuHJzRp96gEo&X-Amz-Signature=57ae43a9ba0883aeeb7bf77ab58de95559f3f14e8f96274e5c5b84c33e0281dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
