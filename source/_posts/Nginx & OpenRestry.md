---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664GUBURXN%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T130043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDwaCXVzLXdlc3QtMiJIMEYCIQCFgoeUMi3G77u4oMt9q664KFC6lFQ7FtlmHU76Ty4GjQIhAOsbX5mVqUPoCdFj1rHft0AppkEs1lj0Gz3jf2Xi28NBKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2BAGaSHNDGFBq4xo0q3AN%2BbU9ht016lq012wKcRJ0YTRjhOdI1%2BIZn275BqJZE%2Bky6l%2BrYrs55KctXXazCxr7ncleeuucuUWws7g1oTy5Tx04sISTMJVT3R45C5awRxsXNfjLUfLg6Q%2FeyKOokU1EThoy6GpI%2BZYmY6xs%2BK8i0nCywprTjzDN2eA9pBAAeNMl4VWlGv9UIGbFhwN1TLJ%2FXqSUOLl44uMlp7Ld6tKOo3eJN8VKz%2FSGHEe5lN3CMFoN3fYDTTmwoSXhLR5lhvBUs85VqWk4l%2BkJ%2FuPPfNbqJU%2F6YBu9%2FOoJoYofhbVr1C5M1wxTi6CBpr8mj0NnkFjeeMLoAGzTKSFxxLVlpGwVyNgFaQWkQBGFOOdB4%2BNa0f4rfYcLeOfTjlQYcyNwCPEIf6%2FZBDvkJmT9mp9kw%2BK5KSVqNc8KStzDhOUxUFUJWvX%2FTlkK6LPg74rIvgHmWfnTo77bGagPhC5pBPNi9dhsrZGw73K9Kx3UIfkIlrxkTCGzzkhqeUjgY50C8sJCHeo7r0ofERr3XU6Y3oBaBvzq8qbIzGLfXsBNubC2L6krxTMcTdfsWbmhVSjIRn9sYFTwqz2R4ibn7Y2XFl7Bij28PvLA%2BNVhN4WpyIVMbubNvu8ipVgwyPE9hEPFxxDCdwZ7HBjqkAdnqGg3BRyrkrw8G0Lo0oWtLrX45QM75%2FCNw%2BEZODEDWlIVM5yWuoPnwAlzqTT2%2BbN74hWfz7W65dFd5fM2eWqw46ZVF9dT9xSrk%2FnX23PVng%2FPG%2BA3hS4kdWsFlHfTxdcrhJiQXoY5BHtS%2B9VNcbr2b3DbBrJayfv5GrmnFOlBgBaBNhN4csHRFdnMvvC93YStPAVGMtlt7z6oGOEJCTWNmoHFl&X-Amz-Signature=7ef95f2df4e8ee218cdb94df3ed3ae32232a4b6e9d0de9b1b347b1edb05a6f3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
