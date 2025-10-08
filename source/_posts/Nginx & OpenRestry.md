---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RXB6GFXB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJIMEYCIQCEiEqY2RFcJaHYrWWWwQvmo0piZWLvwCA3xk6vPZKj%2BwIhAPR6hMwXz8LNkG%2Fw8jQ7ImvYecsPAK%2B4pF9w9dGVjAIsKogECLv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2FGlcfjI3znY2DDecq3APgI6mnkNgWC8X46VH%2BEJjATqGLYB4TzXHvbKHXSuImZY4k%2BGQnxU918v2XJYlh1fS8sNJMLFy1CdP%2FZQW3KqKsutRivQoaEhGYuLZhQyNY6wsGlgox3z13X8XWMpnAl7tTmXBeXbGY25lMvHLetibsk4u7kN8e54Fg9wJKm23LQn9qyEFZyYoOtAC3Zh72praMIXxQ%2BEXhBMvQNcNsikcFGxMLZqlw9D6kz%2FFKxzVaGrcTdQJemO2GSJi1fQ9HyHEarqgLoVQIXNr4i4eq7QFG%2F04gld3PvlmRu53fWXLRcZWCnZXCP1ShpLNY4lnGUR57lhsQoqinT7UaoGBsBqoYeObJSgZPHmh18rE9fQ5ZFNzvwS8xMrC2qJ7QSbzJY%2FvACDMaSE8SY3YXKJmXMwmZquEvrjrDIG6am27OTv1GNke%2FtEMTZto6K0%2BCFpS1eoKqRLuceEkljNvsksGynaXN6A9S6nXGtWZpol60glWjiBopv0pjx7bWm%2BqXu8T2qrPgJfuawwBMv8Mo8212rHgsxYpnp1VG0DFm1gP1luadjIei7Szf57dWqCv8ItOrUmbuubZmFk7mh3k7%2FFmOF%2BNcoK3IMHFp4VycwbOO3%2FsMrOwUSa6at9liyMmfNDCx6pjHBjqkAcZul7uUdrOQbLENQ7txXEJlxgTBKI%2FgWr55npZYDc5F4j%2FO6d5S%2BnUNpzb0Xq8xUGCAXD%2FMTjpjcZ83fxU1S96NtXBF7wQU2%2BtjuyuQsuDg3bda%2BKFOBPK%2F5goNzoUXDI6b8QgkwdL8gEw7kKigYm30fwA54d%2Fg%2BIXQ4o8m88eIpOS3rlI4B2H6lsyyxX%2BvPK5dsnHBNKVV2CpDAh4Wzq9jSkNc&X-Amz-Signature=975a1301df683fef985a9659d995c642052c49ef9b3bac12852c84ddcb75b31c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
