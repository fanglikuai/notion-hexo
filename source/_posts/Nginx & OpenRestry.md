---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEC75CB3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGoAaE10S6AryU0VW3O2qGE34sedxXClT%2BkkCOGeiJftAiEA6ff5%2BGL0IWL%2BGmkIqpMZLS005qKpBb4XeDZf6BkrgHIq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGDbOxXZQmb1Fy6RRyrcAxrrq%2FCGFdwHx7%2BY9fQ%2F4h%2BdLygxRhOk1gD%2FpWUT6ooZfcVUWSLkC2btQoesOOGV316LyC6cNZRpxouyXrx2X2xtvj1uyUL%2BlC9LW2OP%2FqbOfuO9zpibqz5cmf7wZjyQSC7vO3l%2BHToRVrqn%2BJTqa1jBIEy9ClAnvEfwItHhbwZ4LaiPYk89HbuNh93ZirHez5Kso0kG%2BH7PW8m7Gdy9QfV5UjK5wCaJdZWCE%2FHvSlDrGFIzFBqrJy111JxI0f0vdt0yLobmdAarBWsIwrVarhT4qszApLEMPTa84RKpzeUWGo4VJqJZ3ymIDebPVn%2FlWJLuPnB0KT%2B2enlAc9fdOA3siRrRNZCnqxecAM9K7%2B6X8FkDfYmEqyVpM5rJWtY1O9n8pqkYMpxPOEAEHqCDAysLyjUKvquJ8rcyaxN6WhldxejxxP1V2gMdnZrjJSYqdBK5De8jnRAsI24ZaOw8oJNLZZpC%2BZIqs4NPinf80%2BvS4Wb%2F2mXD2%2BUl4RS7uAKrV7s3e1CHorfbCche%2BGjTcfY2mOGQsz%2FdHxHehTEZdOWovxQ0vfmpo%2F34Xg6OzvVMDpyomg6u%2F6Xi5wXcqgQpvcIqoHJBQXJijizvwHMvqoJd9l4Cv2GNAIhWTFogMM311sYGOqUB9mIAYbvl3gqZR3OUfjEkX%2FQ9hgW9S%2BLYFWBxxbZlicVmmRlQwVkJ7ViJPro7%2FnRogSCncap5tcNq3J07Rg8HBcLnhNED%2FYf4TMGL%2BVFJAtT%2BOZAux2AV8L4tmdsv%2BzWiLt2djoVoctf6nSu2NHgjaHD3wxqoW0%2BFUq8J9JFaCNmltx5P8k0gizdUQjwM0Lty2VPyy0GbSWPpbtEXdM19NVjUe28y&X-Amz-Signature=0c6b0030f16b2a1f404277ecb05bbe09e0db00a78fa39bfd7d4e0a427a8e1deb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
