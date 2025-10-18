---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQ7TZF2D%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIGaZ4eMYpEuA2tYT5vE6%2FqvePqIJwYMq9NHTTWz%2BP7sqAiEA3RVXVgAFejrmJ4SARc85nHGbd%2Fkdm%2BIs%2FyOOL3zN6uQqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB9zKrmTM31xmW6oMCrcA0SEW6ckyo3luzHZOmylXSW5QT%2B1F6K0RHMVrXEz314ISBtawNtuF6auipnvUnODfTmIKtg4MwNC6jo3E99iSyLR88zZVdhpt86BiEnAlV4rdXgRniCHqnE8bmX1JlL53RA49i1QhOqiXe5TmxU%2BBShNzGfdhoAlJnDVjBOnI5f09FmadUq50YYzn%2Buuo5kFV1t0Ea1FMHgpDK%2BRsj%2Fv1UXXR8CejTMGrsXF2ab%2B11lOjoL5fLwjuK9AQo0VKPwbhW0LJNpN3sub27hWsAGS7RYuCDTg7c%2FxY3rURGEZFOceitJWROH4LHLRFGnMc%2BzSGtY%2F9Ka6iRCC%2FNYmV4pr2qOGIGpal31XpcFVkrqty0OaCCdvUhwhD1XePGAAhzq0s2H1zT3h%2Fgp%2BXCEg4iaYWhuqi%2B%2FUz7I8o%2BM3Qr4Mk4o0XWcTwyYveIiHctRAV0L9CN2y1Ok0xVD4DHj3WSz52KF0Lj4ouj%2BI9vYnS8FLEFBZcxD22%2FYxv4c1wZIrm8j51HB%2FuYvPov8HlGqd5n8KTNXLpBdihWGmD2XrIwz%2Fy2Reu3ifBgR2WTcjicB4yYSluP3gvkzWJY%2Bd03liqqgIDdBeJEv2vcqCPwdzkAIWDOwXwcph2vWuoMRNSOYnMJvIz8cGOqUBar6grWsH8Dl3deEIV7qIkSmlznWLkbk7EZ9m1m65l9QsQ7a5N2HKrMaFzj6jDf1OpNdCA69yvpV0fmrmHqBctKG1JHrf7F5w1YmyyKjxPYxXXyqmZXN6dsoFeFS%2FZ83nFL3HigNomEeNjJHRv2TqKt42md4B9%2BXAaZkzvZX4fbMSreL7tQP2TiN0s4Uw8jFYZiUNTdR%2BaD1BDX%2FU4YwUcupL6wTi&X-Amz-Signature=2301cca054d72afb8af1d6793875769e00a0493ffe084bc4754909b1bc66c656&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
