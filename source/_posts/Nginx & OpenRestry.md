---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVKOV62V%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCIHlC9XBvuYVocBvrpTBp3q%2F%2BQ%2Bp%2FKEuCSikG2JtbAW2LAiBaPVvaAFtBSzIaS7D1IgCHNy4AOJpDbsAJat46fIHqxCqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAK9t4cVQBTGh4nf4KtwD%2Bx6dZ3GR%2BnM9HR7scwNYKhY9iJ0RVuy6XpNEUTouimEp%2FrfFT8%2FXSmuuP5LlBtb8xaRgwlkI3p0hJPGz8eeg%2BYMtm4KWeHkSIn9c4thkDZgb0AuxiW8UouSDBIpPinsQ48fBI6ph%2BsgBiyKbqyudnsLwGqgeJzVixIeQ8l3jopQqjZk2Es62DsS%2BuxvkjhzQGqnKAOkEF5aJffzJNC2clnCAWUXBQkhCYWiKpz3rpE1SJIdA9xDOKrHJRfqk1eYeanvBfkenLoaP7NDnEX7GErn%2F98ZbbpgjnsMpfPJjtaxwiU2UnSoo5J6Rv2KLunhPKuq99xzrO%2FcR5N2ZVIOqFlOdsMwkI3KnWpyaskLnF7%2FEoj8OlrxLE3Q1FFT7v7VjLsD%2B0J5SO33%2BX7fFOJwyCKpaB6weS8YznoXoXeIP664zXhj9Av1V821y2FMZKb%2BoVN1JcQlhoRca7wUgMqjfO0fTq5pV5ZbGRed%2FyOlxPYro93mx88heIjQ0YihQtuTASCqTBc%2FrIcrjo2sXb4TUhQP8R21iHWGivRLr4TyeSGfJia3gaBQ2aG2SxQA4Wbi9r6GgzNmsq0uNMLyQCOfGVUlstunkPJiChUi%2F6lBhB6FWZrcSVdlUxAaVUOcwgqn%2ByAY6pgFfBzd79MD8Md%2BbVE%2F8BummXldFDOImjO%2FFp7O47lNyZZcgzxgynOzYyBjg3k8EsCFZshaPT60Y8FP8hNvGgmc5BveXmK6bQ413JVsJ9JWeBG%2F9bfk4DP3%2FAkbBMjS6hHmOGzrwbzaPjLXa3Fx9ZJivZLVL%2BrihkFLwkhPsLYOiUObWfnSlgosTk5V1NgjK6eAAxkJaj1EtBa91IqjRUDM23Ii29m26&X-Amz-Signature=8587451cb6eef5e000b8ef6b974f4d203ddaa807f8673c03cd5d67fed7297312&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
