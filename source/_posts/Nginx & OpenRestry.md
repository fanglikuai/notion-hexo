---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XWJ2PBY5%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3mLR8c8IawyNKB3LBmUAbO2Wbpyfw6erWmDTwbqcWKgIgFGmtaVnrDxM%2FiLIKNj0jS73qchlwBo8ZnhdQnXvB7fQq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDNOHEmOrV2u4J9An%2BSrcA8QrB5J6c4Axh7w8z7cwty2P4q6jpLEHum6RjQtpX29F3jkn4e57tX418BHfZdoscgnMtdUYZMWuyjjQ1qDKFWvc334PyjQWQ00gIi2I3%2Ffm87O7QQjz2RndT%2B10qtd7YYjMXmvdGcnNU0sYBJVywxXhGrt1j6LPdpbiEQqhCH1iPpIcrn9c6DpJFfEgO90tOwfd%2B8inD4ilUSpcyUznE%2BcuCenbfv%2FPJVwKk8OKuXX6vAgAjON3Htl%2FA2blO6GLSuU0penDLG5O9K0dpF4MRWB3AODmEJgpDc0Feqh9MajobuHZ%2BJhuvKi3bPYhbc0gTtz4wIPk7Xq1%2B24KsxLeAgIwKRVgEoWGn%2Bv4jUseZ3BLS5FM2Zq8Q%2BGLdij%2F460P0hfhiLaYETqQLnnMsJp5bgupYkUWhhvDg%2FTJXRhbnwOHk6Gh9GMWIhwk7RPoqhtqAo4KMK01Zky%2Fuo2UI3qpiFNmeFQ5gk%2BhGgW3G4nMsKbOxzpCXjTUYsYGbqO2GJxKXjFQX5vj%2B8PrZFBQFG50BkCpriQKjQMU9fV7QNjtJh9stpeirST%2FmxkOA7lyA6I2LKdYQ4XgLGcCxIsM7gwzmNGLx7BR6adndUDyt5qV%2Fj47wClnS7FghDo8wTETMOH11sYGOqUBdlCYJ7Bve%2B3GmIlNLfcWRvwdMSGZRC3cNxQ9f4zGPd4E1bSppvEE9ysDR6V1UGoF%2BNnAt3wd8n4TjnT%2FeXZXnuvT2gK23cNKLjF9WTykPn02XEh9nmCPjneA9s7yc6dtaDLgb37gBHDO%2Fng7ZLf4s6ERWo9%2B4EbjtCmDSmk2Nc%2BSDm3CKQQdUXSVbEpPigIjp3U03Fh4%2FU12ZLdlQsdC2r%2FOdMyO&X-Amz-Signature=2d6da9d5d5efe165bdafd9d63b65dc8e104004ee24e44a6c413a722c83dd3131&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
