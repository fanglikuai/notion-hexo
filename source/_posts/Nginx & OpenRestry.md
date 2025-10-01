---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRIRXYFJ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCn9NKKpOwJMxLr%2B5eXUbtmtrmOv%2FYC8q3pYuuR8EOCqAIhAP1gYZ%2F7bMzfc5cTN1XFsPUbZDvCAa2fTlZbkTWC9YhvKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwhJXkxQkFDVwiqf7Eq3AMpDdjAnginkMF5No69VzbslXIc48Q%2FVY6jWIjpcvlEkaGhvE2LHZbzfmUmzIQxuR8hTEE%2BUG2bwnlIgKAUUq5Y4nkFQ4wa3rZ4NEAxC%2FofWmwb4UUHl4WiAI90Yse%2FDOiu02knFqnRagYoBxobXA%2B1UD06Bn73%2B18P56k%2BT9mh%2BfvxGLqi%2BtQG6T0hIwdnDVakD5cdniWLl%2BtRQzns0zHoGrm6HUyJtde4PtBfYjG0wDuVGmQH5XsaFutZrJtv8koquR7EuxPV81liyK%2FVhDt0S%2BuQefJrSHDpEbMV%2FXSP75vW7wixZjBlKCdr4BBe9xqZ3Qwh%2Bb%2BnohiV2Cxt3LA4rBoclgegKWG6raGHKJdve%2Bynw%2Fv8afvg4DpGg8WxxZY1l3hVstqhcXGHKwECMxy%2Fu6kSxMmandwAMj%2Fc400ut4oLJY%2FexzyyCY9RuQ5gUtajKRp2KThtlYo3eyEJ7kcam0jmv7ksYXG7sQ5IBkvp6kkWCGSpnbm5tLipJt4tqbHKDm0lYbntE%2Bo07ccnVdNzVUk5u7%2B1Q%2B3S%2B9vI%2BSP%2BCxPboYH3mvoUxS9jOTW%2BCmbMoX28O6nI9vYvfI5Y4oDFGwfRGLDa9852MciS5nPjHxIg%2BpJi%2Bd%2F21Z%2FdnTCp7vLGBjqkAegtjYxKeYJ0D6O2RZKh2%2FXOZCoVbKYHot%2B4%2Fb0KiRgEXnADp7dFa4pyetC5EfwMKnM%2FuizMcEJuAy2gvrGcP4w6A1A%2FMrCPTlSK3l0qjAT7EcCrKHtSpSj65WJ7UvPilBccQkKpAnOLkcvFB73lXTneG9AVvogle6xUJ54pMtq0Oip3XKzTwKh%2B1CQkBSX7ftGfQXd2HawqUYa1j%2BUy5BI%2BngFi&X-Amz-Signature=9e0fe037f98c26c91e5427ddc67a4b9433d49e828cf7fc573cd010141e6f6243&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
