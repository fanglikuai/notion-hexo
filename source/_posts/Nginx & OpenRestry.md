---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QEIMEGLZ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCRT%2FsQTqs%2BLmV3KR6UnifXOsLHwyg0FjhDqryhxJ3I6QIhALU3mjQPq2MfWgw9q%2FjMQCQLmlX%2BWr6x8iJF1ujX4B0FKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw2dt54P70fIY4fFOwq3ANfUuctkLqBSgHDMGZ4IQOYRnolI3PsKy3rud281AV4OxEzSsoSeyG7WGsSJGDlq%2BN9chRCA0SUkzDR2POWJgFRD4lx3XQvHzklvTph5wl%2FUptSql7XE77Pira9iUb62uI82mWKociou9W40Cwhuxibo7PQfrM%2BVa%2FTUYpD9WmfKyR7mtODRtV5oIjz6C7yBLWlsap03Ws4IpLF506%2B6p%2Bn8NTNNK9PLgxk3FfJDZcJHto%2Ff02iGdvbDJnYYQzE3dEXnc1N5sPw3DziLwUaZH3wceQZdgfAyG%2BVNRv2S8ORZVAh9DrHIPtp2bBmwxkGzdbPZCnEgWP26OzmgzsWSiUFGwDtvEIb0NfCIWQa0Ee7uheUoPGG6YIIIifQM09BW7iKA6laC8fgOFXPFkoo9Jy7CdQ%2FLDe3tXqWmpHyTWOY7O18uJgl%2Ff3GpIvWzG6YugurzF0wvNW0SYNbGhAy5MCLZh%2FZJZXoQA7xaVpwQlmqb%2B59mpquihbm4Ykp%2Fr6j5VA7mAgd%2B7WlBkj6f9PUG2AQak8Nqe9EZgQ6bvqhj5dNslju2X9%2FYUca5FvSXw40pPGXZPaw%2FTmrPHpXWgaZq35dDMiUctsT50hfw5WaJCoizZkItjG1Nt0GIqzZ7DDwkvPGBjqkAT3akODE9C0XRk%2FnQ2RtwzdM78go5VDZgcJ6oq83wa93%2BWvKzWnmjR4pKPYvTvmGdJGQATPacSdNPudMzu%2FhrgIiSyFBSIrZ%2FxUuvPMuXl7mAvWTYAsZXXTGW%2BKKDDRNdk7oho2UICFLpJK3mszOjydeNm1PMRVILR6z1vltqWffg1632GrxI%2FrY6KEI8JxfS6uJPkRyrnqCMMYoGqB8zRVf43TQ&X-Amz-Signature=bca32927b9db39a004bee2b77f56b4183168357a533d5b059804096f4318814e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
