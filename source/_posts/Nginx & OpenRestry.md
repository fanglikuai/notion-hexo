---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABGOBTP%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJGMEQCID16CfREx8QZ4mOQVnZUYKqtKKOD4xn%2BmjbLBERRQR%2F9AiAY2bSNhnQc1jU90jPi2CLexvFq7bQjfENsTlgJb36L9yr%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM6yngeFXmbCCoyFDDKtwDYJX6wHgOvqgDdZE46LR9JrPmjfN4PN8rqLeRwzPv290o9Dy6y4Iv9xqf%2FgVX0kJaPNZ6RXt4R%2F6kpCfsJV2kOSNs2B6hDnbfqgRM9BY3dNbHCR1J6ZirWiUhQdQmCQFeQKs%2BUi6RYWiwfJHU%2FGnBTaR0aElz%2BLgJL69ceS9HVoiDhyu0eD6UDz4oRYD%2BrhSJsSZvfo61hI0%2Bj%2B0H60F7YhDQkEZvl4G6PmvIdU1wVxu8KtN%2BeVpOpz2AZhGSMl1niqfG9hl0z%2FgYUy3Ke3bEGPjUdejmJWt23ByEUALHf4CwIZE%2BPApNX%2F4CjJQSDBXdx7HKI73o21fc4qh7Cu3QC5HhNEmsNb7r0Ky6bJvqt%2BFGBTsaqa9CHEDwLSmiODiI2GFh5hfCV%2FJCWH87FlDlc%2FhX0gPbALo8OoLqob5wWNSocAifUZ30QIswkhHHcdwEM9yfsyhWv9jHa4luM5hl6fqFQqakd0SuO%2BRx%2F7pN8vr%2FuUVHcX0FAF1OD7Xr0pCKk87144wQhLA8J1XIS%2BEg%2FNkmdkpaRyDb7j95uf%2BbipLslwP5ebtZUH%2BNedY0Qe%2FettTYD9lM25isgjY23PSDda%2BPwWtnkdRGvf9kCiabhOpJVsVVxIuqfcfSPc0wnOL0xgY6pgHzRikk9vpaajYs0yqTYqZFnzmz4HhWK32lAdd%2FT9sr3h7GqtCbBUhBelm9h1%2B8ohM%2BX%2BZAl7UuJhfKjnRAbpk9WL5cfbDVEMNlDxhIK1lJeXzUcVWqyb9qw88QSuAc2KA7ZQfSsWcIKjM2cIw%2FY6y3adykUSueNoGVG9brArrp2KRHQtn0VUpCFv9kr%2Fc1H7c5vYcArqi7dVIs47Tf%2BQoGJHMSFy48&X-Amz-Signature=a755d515385aca6b3d357754a1c4e3cd59d1802fe201ed81e9750b26ba2084b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
