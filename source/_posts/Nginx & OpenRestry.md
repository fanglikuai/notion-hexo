---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJHTKUBD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T210253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQC4gq%2BjhilUiH1scIY1HT8x%2BZdKZNb7U1pxCYll%2BIP6EwIhAI6uX8L2iSIhxo9wzDKwXOdodb942pBUJ1L%2BfEJpmNEzKv8DCB0QABoMNjM3NDIzMTgzODA1Igwo4ShUMi%2BojPoGoPwq3AP3wxNywfGVl19G4ywZplrr9embDndBMqAJ%2FoMyYKTu21h7DYQXCua14hTjx7fNGv0S91%2FH4Cz%2BoOoh9sj0fRmAx5CWt5sCXSq3RVRhdojHjSOSSaRGf8fc8%2F2fdxLOpSChQz0S95X2RT%2FSQ1FvuwwF3pWHRuYR7ZEIWWF9O8DJiuEukI7SGLMlRh2URzBpTkwBLzt6fCQNcnTebDFMy6ZnDrwbUjAledKZAgtRLYwpNfOwPg2cJ8Ti6CgCuHrlUtGBqpNUOSC6iAZqIMTKeiknf0b9KAZ9u%2BAcmNTY1gYOM8U90uVwGmsEY1HUt%2BXr4oyR8vtBVweqEbnT5AyT%2BrfkdL2qVdIV2hC39%2BkMPzVQRW5nbU5FpsALmZcmB4dlFKgcpFrsH%2BIyMo8SbdHg1T6HZEKzXs%2FljSVHee%2BVdX0T2nG7szNJi5Sg5vZftY6pNYGdgozhE%2F%2Bj25Xul%2B1p7mW0nSlgatDVHzdppgwtjkbueSODP8XDqUDMOiH6maLIB0EflZrsmkvMcGEg7rXhFifvdaItqrYZ5KsVrv3MDTpqcPMe9k48EYhU4dJLOgmW0X03rrYKukrrFTXGgTZ%2BcfQfvq4glehdYXGJLp8clCG4Ub1FaIc6nB1wR28RZDDL09%2FHBjqkARWOlFJw%2FY%2FutAzsiazxiL%2FFSUELw2rgTCdqKUJtkGzzI3QscsTt0PVN4zZcLUrEPA9p%2BzS1hkDyCHEWsKVWPycOVwI%2F%2BOEe99VtEPgS2DJVxXFVa%2BAvRsHAZedLXpJOEfQgyHeVZ5VIINum%2Bw6SNifJ7%2FvWLH5jU3mgwjVzZz0Ln185gV08uzp77usxVO1fiTItzvPyDKyn3jhS6ULYm1CCgVEl&X-Amz-Signature=d871ce82262bd87d86906f0a0f53ed04eef92fc2eb6679399ef001b470ab4f88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
