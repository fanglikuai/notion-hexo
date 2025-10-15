---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3PS7BLD%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T120136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2FBVmm2Z9YY%2BP9RA%2FKEIWNF3TWdsfikxtOrpfr4LRUTwIhAKQGlO1kHQVJb3Xl%2Bswo3baXXV7TOmbGjshXAq9BRqbQKv8DCHUQABoMNjM3NDIzMTgzODA1IgxTkhZi9U9m4ZbSCbQq3APrvNU8j7AH0bbOY3futwdi4JO7SKlthQeUvCDi7jk7h0SfbMABv9evXi1%2FMIsgTjeEh7PEWacy2K1VceuaK1GaOMpKVgWOtOBL1UjJCpc4Y5nKe%2BbPm0kROyrY%2BwVVCnjtpXeTsLDoo8ZX1caRK%2FwO7BIPkpEGRRu2zQtf9ymkqPKUbIVoa1kshl85bWjTvhVFb%2FpYnqulx%2FIlFNdXD5FYQi4%2BPbrb43gfG5madf3oi0xTKTDEdugsminhNzEBP1GV2p5FcclT%2FLO1BefhTx4wQi%2FLdzFVj42UpininNwZtRW92xASJVF8h6Ab8PgP%2BZIOaZzrLrYyVwabj1gw%2BxNx%2BZwn9xoPBYwKdALOsH5k%2FKfhrIJmiTPjMqjwTidxO0XiF%2FXnN1mUrne0jyms%2BnD9v5jzRz9K0sSUxKBpvtqFp%2F7XpJ29P7IMa9TuKER0mv7mYO9P0at3D%2Boh4Q3ggah8nMnnpww7n8iyKA%2Bfent2Dc%2BpIW2mlVUeZPLrPxtpXSmZjmFKVuv2fUVa4ST7v32JsPvIB9w7maMaYocKKcouZPoUk2FL79G1eeoq%2FpL%2Bdakh7eOJYVkNaF1RL%2BoEiEja0nfByjrJGgm1wetQFYQFFhe%2BazxCz2T%2BNg7uiDDnmL7HBjqkAYj%2FPSusprZ4UGQsaeh0NuZDmIDEKjtBg7prcJWWNN1iZYeIbbx%2BAuuqYWZRANAr0LgsoWjwodfVOSE45jJGsVsbCPZWDnudz7r1jdacaZwZy%2FA6w%2FNHRj1i9hIqpgZr2g%2FUVzyVsogmBNzICoL%2FX8tv9Zzyd%2B9UaZw64N2eRCVJZdpgOEntOEbIiKW7SCAHbMjaR2HmIExMwNbAoEWCcC5IvFXU&X-Amz-Signature=91b89c5c0d1df521350345932b768037c7aef9458d2d3d9b1729f04fc7e1fc30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
