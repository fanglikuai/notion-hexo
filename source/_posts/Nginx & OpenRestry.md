---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HL2VBTD%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIGFM7XeVEc2fO1f8pgW9%2F1hoJPq4bnnRsfWNQH9S%2BGD1AiBsOgRhoswlCBh%2FRWefq2A5ZXryEYgmI5BB3C0vtwqigiqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMD3A%2B2%2Ft8UPscYe7DKtwD%2Fynyc%2F7jw%2BRmrs3rb%2FJ8NwQ9ym%2F2Uuedw5UjyhIVmvjpx9e7%2B8vy7VG76s%2FW5eNwH4KzmmMcSElgd7n5t680tPkYgOuP4zrO74uy8F0CzXKkhsdycJJV%2FyK1JM7EeoQFISu2oWG0a1pQ32%2Fyi8CkpL6Ej%2B78nEJaFIz%2FMnxoqoKju7kZ7OklabRQvUERzpm9zKMmqABfjqiiqSmpw7ylzcxjlCES1KsRcjPkPiHO%2FGqIw121d0rbMgTbfRjdRLi1brcVaI4u0BQX0rT6EpCJjT7wIUWQ4qZTKabVUg%2BiM7XoAEzN1U%2FmUTmNOTVK0y3BtTHhoDa6Od0o%2BM4HPYdALgOhLakeh6evSsWwSlNlYRZrc%2Ba6qW7%2BEWJoq633wEbAvx%2Blm7fomhBnJwYONoYzOR%2BdmErEq%2BxvZ5yhRLGLZceGSWLo0TZR2%2BJie3Ba%2BO3FXQOhJTIpgBWD9IqKckjUcC5iRHKEjvFfB90ui%2FcvUtZJZugg9rEXwAPVTF5L66dbLkb6L70naOMsvYXWi0Q2nD5J9QigeSga3dY4852sarN0Jqg%2FvTaYqXa2wBbOEg6rnS%2BRjJ0qX%2FoEV4uJLvDMe09UCTlI7UPeS%2FyPz%2BHwXQ2ntiiggcfo%2BwAFcFgwsOnQxwY6pgFzyYg5eeqR12H9YJxp4H7FVW%2FFSYEqfL0xxRpMFC4x%2B5n4edqkY9fK3Tr75fXbhDnVu%2B%2BC%2BQPbd%2FQtYeJYOauKQKn3OfYuI0OIbgTwB3j1kCbrPqzBIHVYcyrk%2BDysp1CdzR%2B0B3jXkFzw%2B1mS9CjsFuYwy%2FxQFDmFeS1%2B%2BW8ujvHaruce3F6kYktvd%2FQgvlRxdAcnTZJ2mAD%2FjKK%2F4Pe6mbqQmevB&X-Amz-Signature=279a6b340de8531aa2a3e2f18c333b3421e91a265927361dd1d763261637dfba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
