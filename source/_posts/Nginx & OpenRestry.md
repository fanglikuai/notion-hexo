---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVL6ZJ3U%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE%2BR9%2F8palb5TmDS8w6jf7jIbTFRQ3Gkbz2ADGHe0%2BHuAiEAwg8kEhI2G5rFnkjX0CAuhi%2BcZwQgzSMNu4YnEyU6ZDkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDIZVi%2FCyi8WOKdrPQSrcAxfV9lcQktcstPwtWnViEhSU44ipx94VSmQH1up45CZtdzu%2FGj6sW8glJCeyFheqRRGzupmIrsDGvFX6eGb5q7ICAp1QNR8b3dUba%2FAdwlApIliyiyEFK3cyuOLLHlWjAFgU1Ot3Jgt7SN7PiG2TefbROCtTh0rlDtvVUlKMQgpIt%2BScTJM7xtn%2FvuRKMwht91beyQLJrXJv6Fo5QCC8Ft8TzQlO05kpBlnP9d5bHx3DzMbQ16P1sWzNcY3de5KGVtAuTgb3h8dT6W9%2BifKCm39mzJnbbSVEjRuOUpDmWDD3Hbz9cG6h%2B9vvVp33MLtTdvfnLManXK%2F36iu%2FtNjvHZ7u481jDWKuaVHv7YGPUSvrya3nsoKC9Pd67WYJ7p5QnHBw6dkHSsLLAVXEtJPprzS41gXAC6B5tiQyIyVAHTyFh2NlQ49zxsjqrAZy9kGuQCg%2BQNySjPTpps0daHyd9CRBuVEI7VycjWVdRjN0TdWySkJyiubKFThyYRui7T2FrShWvnCAY5wOybyLZG%2FIQ0WZNu8WTI8t8B0U4DlslqagAYCzAmYj2ZIstvqq5X289v%2BV3VeAbtAjc3ldLihzfi9rXIxOmaibuJx%2FN7N3PlMvIr%2FSgnS42qe8aJ7FMPq6%2B8YGOqUBMoMc1hszaacepBd%2BYkG4iSNC5fPJb37jzr2QZazpDdDJabjib9pTKKODhtJEPI%2FBmX2rf39tBBJgMUFz7xqdIwLt%2B8TSZ62NOykKhCi3Hw%2BAzFzYgsQ8GG%2BuckPHwDe15dvHBulKgbNxFoY8DZXlI9iO4DNjQuEk8jR00D%2B1KKOCNX2OOmAiKLmw8B0P3UiKI9GOQ8r3jFNhsasDH8nrO74%2F4rCr&X-Amz-Signature=41b8303902a7ce7d414f84aa654f8291f155d68f66a5bd7c13f024b2e1c830fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
