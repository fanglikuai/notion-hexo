---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UG7XOA6Z%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD66Y2SwFD5Pf7i3ITySyPq6u%2BxFWgvEg%2FHVfYrSvPddwIhANePD2UjiS7z%2FMtB3MEVlhW1XlM1wDeaa240EHmdpiNbKv8DCFgQABoMNjM3NDIzMTgzODA1IgyCGyLWaXMdt6YQcDAq3AP2Ngoh%2F78WC57jC3Ei%2FNYNTIF0QZ6nDcUSXwLuIe2e3waw%2Fhn1B27kGJW7yCiFXMXppxcN0Gw42150jYzHnIG4Fjj1WPMW4C6uxpp4CclUsEPJgMOetjoODQpccxdAG759DpTE7H%2FL%2BD6rhCKtgUt5a1GQz50BOb%2F0QNThb4EdcFqvlcwlpTjV3S%2FJqYCtUUDbjSlF2BeC6FMR9VH5d2QQwUtyucNS797u0X7GSOZnlVHLZmEvgBRwSUYi55Fzk%2BHq7jPNH1c0FMBhgFIrT%2FdhYc9bqW5RPoIQ6qWcNXC1yWCbPVdcm7BUehWsI5rBDqKzU1%2BpcKYx5H8D5r%2FpooKjscXLEAqZMKA3%2Fzgn%2FKuF%2B1gai1yXwDqguScYDCksVVVr7ITSebmn%2BEOVDDTfvWcIucoS1N9NMgAWEa4GqT%2FX1%2BZD8RA0iLzbIGvl8foaOiLc%2BMH3HUeNzunM4XnUtXx7HWoDkbSqG%2FxsS9qz8Cguj3Pzar%2F6Jxree5FMLkjs3Z6BLSG%2FD%2BokJEuZv29NmLAj1tcCuxUQ5XE7xyFRS3PG5TqUuyR29HIlKnhX3XQJO5znnFCdruXyJH%2BcMAacfXFlVrpYwAWTv7FlUX75Ny%2BG2liaesWHCSXAiCmG0zD4rqHIBjqkAeL7FlOiz331iEikSRj8JGJ9wa5HYysWP8MczDIBQW0BYcgZ1aDl69O2QmDaLiG2m60fNcg7pm%2FSYfS6tq1ng5j2CP2Ke4YODXuze1FBPCkZ7aOKztzwfchLj7NKTqlVBRxFnyfTmpKB4KGUvo45G5dZRYVU2iUyYiGjk0AzlC9H3zC7qDIV9K8CH37Ogg1l0XFyUrv613wCb9O%2FZ72vOpZUKYr2&X-Amz-Signature=9a2208c830d6f42ba93df2d0cec31fb5dfd8541a0b6ab8ea64fd4d33e883a236&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
