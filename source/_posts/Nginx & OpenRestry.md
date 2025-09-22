---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YW7BXXP4%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0xXVN3YYG2ibkJW9bwo3pURfh7zPJmPn1SSI9gFkhiQIhAOpX2PbK%2B0h8%2BfcBEnZAaIVywzyhBMq8K49Ty7jTPu9fKv8DCC0QABoMNjM3NDIzMTgzODA1IgyQmUr7zNKC%2FLp8rqsq3AMKLpvAdlPIVZpiD40jBZg4bXMncpsbjTaaCnPPox7O%2FmzGEruYcGG8y57Xm1W5eeCFlSOhfclfWYsyM6GDqvLO1RkYihKBH7jykVzhcAT1GzOZULttNJxic%2FXu9vWFhWKOtnJTMusR3aRnVID%2FluUuma%2Fo8R4kbfdhOxkfjdpD%2BmwtVpbr3rtyq84FawBCkJkcXrscS07b5SvJ%2BYAfn%2FU3LjjRH8QTXMnz5%2B1ck9oPRpn8TEdWF7rPg96VnT2thQ1n9zxTff3lMr0DIYtsTHJ4KZX9dUydHNG%2BvxRI7z%2FFEo51IVTyUt4hS5YDvUKG6%2BFGn7TjSlB6Oj5jScIP%2FInA3wrJswTP7hVtPJKnkJWl8Ifw%2BTX1U2p0zOmbO553K6gpYmQMWVp49O%2FUoPGs3OJUAjLhGbRUBoB79Nsyt24CyTWIdXvBEdVLmtf5LBHkJBbsNAV%2BAAyxadJ4AOBDH8QImg0evlXSAaJoE5IPnunrlnwgoUhIWUCe%2BrPWB58yzNgth54wkfb11fKJMccnhoduJl2sxLVuhj9CUYKDlhfz6mGh0M7hbMjrV5I3SatKo0uSaQmeSV%2BvYQ3lwb9HsSqS2QL6FJjXV%2FL66RpErXeM45S9YQVbMX6%2FXN8KXDCo7sTGBjqkAWnR5RU5lIfxd8JKCeiBDSO58WitAdV2oZ%2BTfnSGUs5YvNkZeyo%2Bd7XIPLENoUap9W3dTbggST4xhuKRYpAJjBUN8VHB63JOsfUEkIwe4MAdD3QIgYnuiWqJLgp8FteEkFz8PLV4z43FOP7%2FcrvBK1otNdtiw3TEblup4%2FEb0YERKbkY9rpG8i8Luh2pKHQi7CA7JCtsuZmivm%2Fu38wIQ7w1jK4l&X-Amz-Signature=cf98224dca47f80258d0cfd02be92118348df567c65ca855d034392ff434d2a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
