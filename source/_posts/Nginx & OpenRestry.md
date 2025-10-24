---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ACUSTSC%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC7tQHiUK7pC%2BC4MBiBX9qrbxtj2AV%2B0yNePvZhP9dkkAiBufBdsABck5nW3eN5RpnUgMSF9W7G41HJsGUwFeaeS5ir%2FAwhdEAAaDDYzNzQyMzE4MzgwNSIMZGc764a6V4PRcbC8KtwD1Cg%2BVs5ZAhIw4%2BlYqVSK%2FSnsmyszyUGHKF3Vp6LKQ1E5bYjgCT0vF3h%2FDlUzIp2pCPEmlxfwdQpVK1FSBj3Aa0NNeoChisGhGGvYK9k9TQm4Xw4eWKsF4vAZk%2BLU7N5sC6qFvGM1DfBx959iBIJW7GO3K%2BQ%2FiLTyOxDm35vQg0qm%2F6ToEAGMyx83Y7krMZwufMVPAuSqdGGaZaRTPqeWnmNIqEkFtnK%2BQG2Gcy9aHcYFMe7VrYHbWPoUr0E1zKqNYJMmUoT%2BucS8C%2F7YeA5XIMk00rCqRCt1RKrX7kJaLfeTNH94P0%2FGUhQ0twB3RMg%2FtrXb3NaNJYky33HOFMbZwZPVOR6vi4JRaThyx7AaKwavzJcmqyxYq8CtOxYOKIRMr%2B7o6MbpYjDdVcSlQsySjyllyCR%2Bszq%2BXJjt%2F8D0B0Ycm%2BxAPVy4sTkxIcnbBWTHhKIUOHYuHzCkyG7xTPncuI113cWWmcKad6nINWsY9a%2Feo3i108rPjLuX578d91eufGrnIHgtt%2F91yROUynsaPGVvt2L1vZBq4XWhfSpCMUq5ZxWgd%2B88xUEsAvO6ZnEcZrnkH9eFRm7CW5ybDhIIIUtOR7XSXYcWfAFVywepwtFOL43%2BNRUfvTktRsMwjMftxwY6pgFz10eIDl6DQzncsHFqyRsnpe3C3J2NOqyY82QbdwDn%2Fbt6MZOzdYAJfNVwvB7f7pld0iAIzgWNG8uCy6uYeoJ4gltM2yfJSPKTR9CH3PSgE4wEHvexb02mavqMnWb5RYx4CgZDf9W4mxVA7q5Nye%2F0iG423U0iCgrr0QN542btkLQmEaF73PzQbYvpwo1vjUcuRifVvg%2BfhWQDs2cRMn0ce2I4%2FLW8&X-Amz-Signature=deae915c2876b9995563826b1b04238fb780fb9e5e8a0b83962015e848e5ef66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
