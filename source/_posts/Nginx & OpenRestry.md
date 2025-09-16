---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5THXP7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJIMEYCIQCwjLCSFnqqouDLskfWez%2B17SrFfj%2FYjjFyDzycVgaE4QIhAKtVPw47FxB5XXVLlQo1OqhgBJ0wul11M906jqUhGeE7KogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxvitD0760er2noqlAq3APlxiaBF567IWiZEvPg3XgaFPLgrX6lGN6XADl%2B3174KPKbmM%2F9uNmGv0diEi4F%2F%2FLKGUeET%2BAy28%2FPG6%2FQEW0DOpCuBaKlTFcAqC32NlNp4ZzKik5EItDps%2FR1n2w7ROCo3QxsSAMGLs%2BzDm0F%2B6rcnQg4ql6YgWAMGpgMzBGBhnbyl%2BMBH5r4JKkk8D3%2FcZ8a5%2FEvJ1CMJ%2BaER82F79kJnHCnFwt8yXa%2FB%2FoLULhdNBwv7MVyeG3CWdGFevPtjxhfRnpZSAJKEaNJYIkUFNCZyqIjsbTzkY7VbX18qeWuI%2BbSi9gpbW%2FxtFrM60P8ewkzwHtxaYW3zu%2Fg%2ByVtdpA%2FbQ5eR2CTfaKx833%2F175Vnf%2B4fb1Ps%2FFUHTdvciHq27O4y5NYYI58mxWDpJDPjsTiH7S6LkhGT%2F2rmyDnOqW5JjGUAjSzq%2FzkPM%2FNn%2FDEbQvF3S4eOs5ol2wEBdUu4s7QclNgAg3RnCtZTGJFai12SS3Q7f21yntwcGgv94slp5W%2BT4WtRqbl%2BEs63nj7Zz98eAjIGxWSj2evfs72x9DFVMcuf057jZmkxa4LnZV5dPPZE%2BQ4xpjEph5VlL7gi%2Faj9YItNwz7SCGe%2BBPKdYv1FhHIP89xEOdKf7eQjzCooKfGBjqkAZ2AnL3VxmomO3rYGxqUrBoooYDIYmzEJWE5mZ%2F8Zlw7gIutI8SBfnOPLtbYWTq78ln%2BKJLR9Gm9zi8Ir3iw3FBZB%2BCI6nUOgecUsnenHXHiQ%2BCYLXTsY996a%2FHWPXV1hxbQKML7wU67O6BnL92ILN2Kxq3yHvhOm4Gd37UfvgQkWELFuT8sjNWE2KoRhq0KyEiNqUfeMIm%2BIxdAFS9yGJ%2BtyiYW&X-Amz-Signature=ef161751effe01a1e0905926226fa1f178079d1ceceb31a45c880843a5978be3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
