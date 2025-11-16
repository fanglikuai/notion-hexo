---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T4YTHDUX%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD8nGYQQnzMNYYZUY3QoU4yd6vtZvw0vvPdc9XDn3ORUwIhAP2uDId%2Fu5OsIEszWVwE1kpWZx3S6doYLHCHcjvfgS3%2BKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyphl9lkA1s0q%2FX0kwq3APoIBb%2BqnIvxI8Ohb1mRpOO6pC5SLs7limcQirt4SxJr2DE1kEDDhFt2hx4tcd7nFPQHlsjHnM2ahT6ChSlNoCzLUcq9tQHV6UrT5O%2F67AV0ST7gSWQVM871rEVqnMKywUJwFbBogdvvPWZ1550lR4YdBzhQ%2BlIuED3WHEWxiQL75AhBWrYPoymSeqFCeS1O8ch1j8ZE4vZ07VrHEBpEeUwM%2B0Vw%2FHOF1kS6mrFvc56OR5p8tmMRqCWP5HjrECqhoaBW2xFHgL6wnshT5WBUrhjfRij8vDdF7J4oKLMMcl9k%2Brun3pD7b8yQrGpURzwxCll3d5hWFOqQqwOc0z%2B6mhsZ1j%2BkXLMPVcIj%2BAJyXIjIztiYSlq1Uk%2BDJcIMQ%2FdY5y5ae9EduTr4JQ%2BodwHJwpK43sL5HwYbrhgW%2BGBivy28aJrv%2BVUDgv%2Bna4jadGfVSxWW7jZZrAzbMRshE%2BCa8sSPHq5hoPq7vzEKFxbUyAkQwkKFiI2g8irubNyMlFq6Hy%2BqBYWUyhYS71AsLbkb9hwnH1fwC2g1mXX85znvtDpSMy5V9mKgMULshZEK1RP4TgmYyWq5aOcMQhtWf8YQz8pFOG1Jqbzp0Xxx5MePaYVovtfyO4vmkA8%2BWbymDDuxOPIBjqkAZdOJKbRhWyzHQWGRQPHoboowzYG4QbLQn0Bhypj8N%2FXMQ4KAEQkUZjvqh%2FtCRMT0gW5fF58RIzlhP88oiGrAz8gpJr7IQF1HWrLCuqeBrSb47CW%2F742nq2KT%2FUDr8Thjh7Hf8PbHau28rP8lwcuzNFWCgZBeF5KVGUZaJh%2FER3yuFZ1cyVMikfh%2FyRr%2ByC4yapA6%2F6MW6tiBNrdAeYqitlxIqbD&X-Amz-Signature=07520c13fc91f9b79fa35d04d71a92d9a33a21c7f3078e43c0c11db63811f580&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
