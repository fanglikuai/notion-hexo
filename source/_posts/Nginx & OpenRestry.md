---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466346PR3IF%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW7tiKmLbnhUs93snAz8%2FQpP3E3sao8fEnuo09vzWB1AIhAINDWZtbuC371hHifXKtKYbKbqhOSbzqhyFzHOz69n6%2BKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx1h1hpU4rx0mIQ36Yq3AO%2BvftAJhpqnAhr9TiV%2FTEBP7MuaY64ANn8PWnavZ4nxEs7pQ3KALZAmJTIN9UoSw7m1V1QUc%2FLZ3e1qnwo6GEkmkKSGAmoX2A3SA%2B4hrSytrW4dovUnsSjmpN1%2FxfIHItRQdBO8NtsM0Nl%2Brb5gBuedyrsvkCMcp4GzSX5jMGD8O1aX%2FhfzJJiVVTlnNc%2F0GFjI8nBAf1GXbRTXygHA1c34SYjYlmOWBq00GeUbPpdXHnN4kQ0jH3gEFFd1Fdu6wfAj9yLfvhv%2FJFhu%2FrTv4KEe9tC6QzXyjBHRQ2XlofZXAeimhU%2FgQ4W4DVYELgoj8NPa82inck1VzBfJKQ%2BD9EPLxvnLcbTt%2FkHEg0INlGCOvYIPCKgTSFBmEblrBFUbms%2BsvlH97tdTcDjJNPQ5Qmu4WrAMYe94JEpFZh05AY9FalPBKHaCNpO1RrSIoICNxtVBKJ%2FNMScTZXurynFXi7BcVoJPdMv%2BVbtOlrfYgpL2njk5KFHZm25AnLJogGCZ3OSgEWt0N%2BYPiw8m3MwaFXzGcQ49b%2FJY4o0F0YwBqnXRk0S%2BAL0KLJ3FpbE0ADzmIXWD1ie0xAPBrz1x4riMqTt9ti%2BD08LAlPtcwYCoFC8ybLuEug45zdEri87EDC7mP%2FHBjqkAc4UgIQLVKWpdVcaUK0M23SqM3TkLKNCpiXODI1v1Ljqvq97zOKF%2FUki8QYu8d7qd0gJEsi1%2F7poOvGpCYT6LJJo8UIdxq6XEWRMHXSzZT0Nh2qjKMwIbOD8NoIN3ZZpFjl7gdjVNp21opaYBaNp1n0Q%2FE3bb7erMgnY1ECXPHK6Za4LdV7F5NdWLlvduX7ADf0p8Jq%2BqRHXrlOrHclX5IYMGzKe&X-Amz-Signature=d64c7094c2777f51c13b8ad5d298e1daafecd0b3b1fd96a163e5ad2accb8c092&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
