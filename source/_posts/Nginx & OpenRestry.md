---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGOOPBFB%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHA6RYoaYBNwk2g1uyu9S9jrN9EI5JscyE7nd%2FPno5jzAiAYtoD0nUjbLa4ABoOgkTyNz415ytJmf66nT%2ByFDc4h1yqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8IvKmjfSYVKRfyDhKtwDVObU%2BiFSlHGcpA6R4gzdn2bfDmuKlAf8HXMMppSGit1SnyUUeTILSnDInRq2w8y79gPNTts9cDVaH%2BCvxdne1kgWfXE1tKg8U6fHSomWvjGPc7XBqr1xnUFO2oigxGi3cdOzBA0hE0TkP1m8XmajL4USnMVVUCUAj2cNWPT%2FEkRDke4Ku3%2FtOtwDiwWRxvi8PisyL7RxSL6G9GjwpXS0Z%2FdDLPrKlW8t6r9ppFHCBwywe%2B5n95ijC1vckF7FEvxsRFXc5eTJ3pNl%2Bs1JsuWV4igHY0b3AKW9q7ePGaQNy%2F5PZqdzyzEiwMYcVKiu0bbd4bHzGeznHL1sZPSUGbFafvIgDmiiKDnpSbaEojvxkHb4vGNbPqbtQdHpj2zmmSOKpK2J5ZXa1iyn5%2FamX1Z4Zw3ZKsSQ7rA01ZDVr2lVnpfCfrZLz4uNdyHQ4iJMxShzvd8jSoQjL0I17NgI6fMiVrGxeiWWY4sVXjohwzWlCCyuMn4sJyPj2M5TFEOKr0Zs3zIsH2zE2%2Bcd%2FYrj%2BkuMF6uks3EfnnD7zC6nNxVzM2ewue10Dt3zkl58ZBE7PPuUcCZ8SpBNUob%2BzIEYHGo86zahM3cgiG44hPVoKU%2FwnU3FsZK5NooK7ppZzTQw0tTqxgY6pgGok1S4Oh%2F9Utod%2FC3Z%2FZFGNHxYYEyJ7XSTUO1jl5nRENNYRfrelWFE4cgsq1PCPYSrZlEbyZGi5FKBEIWK8PEMhjuhHbjiuukfbsMR%2B7563I%2BwhS8%2FYUnDLBrrzVxlJx2ZO6hckCIEsfsrKLwPldackKozlTYXomy%2BjW3IyFc92YpSySqSMZTzmPVafCWmn%2BmgQFaFE14GM8PNsCYriAHEmTWAtgAu&X-Amz-Signature=4f79d9137dfa2e6843271094dd83a0ca1ab8c850faaef2a38ce2b55393c07ada&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
