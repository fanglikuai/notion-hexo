---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFYF6K2J%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFzpL6anOSDEQPA6iRZtaMQYrRfL5DZ30X%2B6PfB4CxgwAiAnTlmAXYJ%2BGVWNpYrFBV7tIQfbWhMFzgkn18ubKZ0y%2BCr%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIM3N3Dy6wkB0%2BfQ8VWKtwDSIrIY1Ms59MvvbK3vGj4ogbrGP1lpAz7CJ%2Buz%2FCNz4e7aIVv0HrCmmQ6kBwx5%2BPPh4nuXmX0LzX8ln8AuI4Woy9sF%2B1ootG%2BK0hTuDp6F3hS6RDJKgq0t%2B93n0u8vyGuMlcmC5sB5rU7EYhqFNrB6ll6dvrjdk5XbVEsGwXtquRKC8N9Rv5noyTwEpOHQ08OtATh%2BxrurfIh0zbg3zQkgmitzRlFCKQTZAShT40ijl8wgPPquy7feZgWxLq2cWGVqoUrBJoLT5o3naqG0Ur%2BI9EifI8MZcYCCdM6GmX2JrYOOAUdD8cIcNTHOsQOtNzUZectGTcpr%2F3chI5rdtOi9Xqi5wQUCkKsABAx8iyWqBbx%2Fiobe9mZd8rtOdciUED8buGa1YGO8KrZRkEmbIpMU05OyOWjfkJzoK3VjPUAy94DnmzPPHnuxEpT4u95cfRR2KUEd1wAoAZVvfFaDtFg7Hgd90ak5AvD3SCdD1pV%2FwDGGiTRge5V1S%2FchtSi6ecfiR2a8mlU1GOv0qIcIobOGJnYIt2x7CIFe63ApqEsZIVMKGusNoPaxNBrvFvnlfHrmH7vK1jqRTNHS6ipnmUvyprWJX%2Bn4wlejxs6Wc60wIvSyuxn986ahBM%2BszMwjdqVyQY6pgGt1K5W1ANko2lKRY9XQjeskf8T%2F3H8kTbA1j7nhl5MwzOBLlNtDZBIeyymoMJKYIc2kXtKY5Q2p2Cz1ukop1RD%2FGhr3440Ef%2FxMjL8ghuWkIssZs8NKEP2oPSCpb65Fxl0wiPXolcYa0rg9KTBZsVs4n200ayQ%2FLAw3EIO1fsL3Fa2gJAjqtEdHkvdaiLWvhriVJdU88%2BiucpFJLzBzfosci9uy7OY&X-Amz-Signature=39cebac575ec0a8640c0fa977b056d4f786d5ca029af3bcc97a8c9c0f622bfbb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
