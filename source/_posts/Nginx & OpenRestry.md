---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626QYNZXB%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIH342%2BmMHkHQ%2FNO7k0bU3uKDIx4%2BiSpVLDvLIc5HxD14AiBTSeE510OIvgC7wIOGBbmN2bRbEt8eCVf6riMgw1LgGiqIBAj6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXdGiKvO4IEgO0sHbKtwDNr5eOzJUn4gj1FAlP03yaeHVS8yodHT66e%2FlEx6CUxkc8jU72ZDQ9tG5Oa7P%2FRYGhaYFKXXx6cL5vbWD4s%2BiKNG8aAEJo3mREjrdfiDYcRtT51srQteGPG2nW5Wu2miez8Pauh2af9hAFTotOVdW0gN6Jh45PTriVP8168dpYeC0psWJ0oT6iK1BHQwbOI%2FNAq0rmvwnOJdFnoAWdQtAf%2BRFoE%2F3a%2B5GrXhCC3rVpdQodactjr8OE6X5JELiAdXOUdrWj2sv3HYBmyCV0URT1Vnv7ojT2kJjEjObIqaN%2FedaGrjxtmFOJzCL5JYgZHB3wJcVdql%2BsPk4eYUQ8nWuHGjE4J2yNaagTCgjwPwZClafJsWxXPqbg0nup1NqnUYNVg16yyJvFAyw8yA%2FcDRvN7tvTxcMu5vcFUu3beN2qJzm8BR%2FXpxZpZdQKVDa8fjHsMWOCImnj562i5hO5JGod1JX9Ew1I9%2FpELrn9ckg57cS6B%2FVFgX6xpgvE1GQ%2FegiiHcpLXut8pfm08Tfonks0PEJVSwvNfUB9vP9TNli6jYzn10VIVnO%2FSz7xszTP5fqmfK3rpoIOwDZNUj%2Bdvgaf75Nboa%2F7Koa6L7mom6pEJENReXuMPu77E2O67Yw1urxxgY6pgGLnHyCaVAortU0Hl8rvfaTJ99jivoy6GofaDAlbmZr3kv4JXDGjBc4UNuofECZDGT4nYwoxgNpsjAgPu%2Bo66Nuy8zfqfH68apqRALh1m6BgSrKpk6FkqL0%2B0RPFqgurAhTmI7cvw%2Bocrzhfz4lUpfQtfUZywONr%2Fg3ObFWOES%2BQCJsA5ULA5rx%2BcbNRTRO2rviFgL2ERzN2LnE6MSqhKO1vNZf5cXW&X-Amz-Signature=0608ae35192047a4293a8db7728f89b973949921a3905f9b569271f88322078e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
