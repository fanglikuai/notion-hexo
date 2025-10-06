---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BLTKDTU%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICkJFv5IdhHQBSLCNWCrnhaXHwv6K6PCWhen0ziDgyX1AiAPKS7ZpgiUfXKsE75JGfpq9VYSdVpS0nWJ7bS%2FwzBKCSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSON%2F%2F90JzEls5dxlKtwDfWIcVSjwC5y9LJHWSJAkUblWoMc3eHHXhaBiV58iyU%2BZjRQDI66GEVO2IdyNaCO84U2jxk%2F9hxMapNS56BjrGgcGuSJfjL3%2FapaGgX4qRlWz%2BJlJKwGZNcrtFsH4PQxlMFf2PtMt1yfZVQWhEgebZUtGTdx6X99BekCOjZt%2Fc3R%2B2vcBrqcM4VSPONVOyeJGvwAwLq1PjVJ35Ia31PlM5EiuxYHpCsmTmw56Y%2FN6nrv1reIa96MBxm6R2Dno2omGbrkhITqLxwjUK9YZede52Gzz3S758%2ByXBEZ9wLcCuTSlYkk0R99TPMDELfvYcOUqeWReYfHHcxzTlkXlvgrbgOp%2BalZQx3t%2FcLu%2F9Vuo%2B7nf8wF8ei0lwmTihYZoL7HOaPf4T6CGgKj%2Bfpc2RdPmtuMYoCexxUe1bZ6nBHBdZuHTtAiK3vJ8QjNRHUQEYuVqNhu95NawKZgrVymnoNL9ofXXlJbjOLqK2YjUYzQ8Df80nKstF41zyiQ1sNRSx11yhsjd5qP9nfvSbuDxZ5AJyF93GdfEUQmtPKGbBq%2BVmWIEcajGNQ%2BEkKrr1sOfZDS4%2F8wkKMizN683iR%2FrKAVAbpYq%2B0ZsjAoo30NNmDXO5v72DzocNn5JeUhsX20wmq%2BOxwY6pgHfofBHlZdVjuIwPuajKZL8rDIZ6y4l14J9cqdcwZT6s65sHywj7ZFFA1Wf1HZnaQCAD6PqbED60loz5V3xjVPDICufVtwOHJVzwDhYDBZRBv9YMUL56qSLjc%2FIQ1SOBKwSceaHKM6xTyG%2F06wCtxvUEDq3m6%2BUlKF3kXZi1t4RdQyPPLF6hsJ2EuHJbvCxsUS1YQOuDGk6tFKyX%2BK3aDrURApMXn8j&X-Amz-Signature=00167dcdb9f9f2e7869445e2e2d793f5e56df062002bb90be05acf812da15f13&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
