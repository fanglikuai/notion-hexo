---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWS7Y4V5%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJHMEUCIQD%2BgBdTRo8nlRtZnCkl6x5L2HX%2BoqbPQqkDbgQpD9sGUwIgErgAlPfEqLLJKpgIlTbF9YUbThb9MhRH2ipgfLshKK0qiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCga%2BsN3hsZYFzEXFircA3rQwz4XnenZmhOrPfcDv1HyERaZY0AgKoANdA5wbV02FDAqXPfvCI%2FEKXIxxmfKqcAf0YojVXB3Ax7MKV0BljGaTdnhUbvuqDKqm4VMlvdg70KPULF8YKtYjzUL4TUd7ih6gDbPYSpmMOKi43LRAFOiRg4vesSM2Za5k77j9M7vVBNoV984roAVrvko86utwfz%2B%2FQR9s7F9ri35nA9QFY6NKBgBODd4yiEB8qo1PVCYSYg7kLV5Fpz%2B4ZFtWHMrBqzSzlCg9aElDntpIbKy7sP8lCklrG77VXUTSojbxENIYLKxoQCNr%2Brvn6eYr7fgT49mWzvZlhngYG7KQpCZ%2B8dGoa%2Fj0STZ5s1hZ5v9zjnatTvg65UfWcxoutWAworYQYZww23NfA3VijW%2FrV6z9XCUfjACRKLIDrctFC8kuE5JQZ9zGSHgfbSxgBhu23wBI6esx9ufnplzPMjroQJWR0%2F3sKkhN752s0Nn%2BTFTAA%2FFofpYoajlNPI1lZ6F%2BN47QyzoCKx7ok3EiJswKxYsgH%2FBZF4ZZvV%2FOGeOoCuR78a%2BYWJbyXnCytZNQzdsI%2BdOXeVv5VgvSDzNzSUeDY2frX3bbA0%2BpZormKHJ2P8JBjthPqE8g3zUDLOh6WmbMIrX78YGOqUBuxnuxoXrvbc0hXtSyzWmWUXHJ7O5ZDMlkVHQYUnl1nrp8qdg%2FZ9kK5PBHLhOPf3Vpm049H8hEDoX6zpSkWoakw1MTtb0fNdYHBy7hvhDjejCmWDqpbFJivkFgLrpHuSoWdf3hE7orivedKaueoP6bcJ%2Fx7zkD4VuD7tJJl16WFCmDMf35NJn1v1BYsX9tbag43dAWP%2B7Xidvamnq6%2Ba3UYJOjlxo&X-Amz-Signature=4fc1d009216b4580fb0e84b5168c9b6c6f55cee79611e0ce6983e278b871b585&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
