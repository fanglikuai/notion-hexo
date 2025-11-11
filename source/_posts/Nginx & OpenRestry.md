---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DN4334C%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQCe8uOkl9w0eBbCW62QUilxsj%2FwoJwTGYzvTpPLb5OXRgIhAMfhbowZP3u3AX%2F9XS%2FdPQCfdgWFhrC3av4f%2BK0GGQdaKv8DCCIQABoMNjM3NDIzMTgzODA1Igy2T4yUZbyfzeSTIwkq3AOA%2FOGmqvuwJk0QIKo0iXvN%2F7eaeW9KrcjSL8XgJT5458zgpCM55gR8Lgwas%2B3C2eOoYocaAVy148011RZA6V43vUwYyphYBWmRp1plDUTe5zqvhUqlmrDeu0MxiroCF4MslgU05ipyUwmDJjuHIyhvwOQZEXJXJxS5Owc%2BfUxu9WmQ2v7qdENt1KO5p7Vt8xCI4i2fYcLPy0VJB%2Fp3qRx%2BS4ssaBJrUD4N49VhAPTpTj8NZVSWIzOUfpOs4O8pd7pvwgYUcyYvhOErZy6YSTRunBYRl24rO7AD%2FsmuZe5Myc6bi5eUrOg6mQwOKKSdKOHC4sHOKZw5bzBxyrPXvz1Ejd1cskh3OFm9DXCoZMWMKmKPqYdOIyhqltA8YnMPqzYTa6qtMJBNBbcv8yFc2LweqBSHchoYmUR6N6RFLHDlWhAtz%2BVI4uQI3IgReUeYJi%2Ba6qzqSqpHrVWemn9%2FUKPSf%2BdcTGt1RRbgMwnamgCkShn23h9Y2HiooRWBH1xnjMx%2FYO20S7PuzwFOmQoty%2FK5ApJk9wlJfMAuSJpGHiWcA4yucenW4dNKtdGH6ZrVgcLCjYK0oYvko7vaDmKQINftBLC9%2BUWQaaLXlhNzblnXQo7OR8MQ5BlA%2B%2BWXMzDp383IBjqkAa2hxSmzJWiAhMgCEsiKqxET1E9VBGgYZgmX2gI7vQWDjoa6REOul5SR6SSFxB9UidiXx8g%2FxfYT8ZPFNLUyjLvH2jTXvgqm6uxYvxxVdUB29N7JI3EUNWkQQNlRDTvMqsqACe9YmjFLh1F7j9S6261XYChLlE0KhAp270fZztwjw962bjzFqo4pICyvz6%2FDOX7zYWt3NLixATWaTCCkQWywvoFI&X-Amz-Signature=2a6366a9f0c570cc10393f27c39b6646736df08790e1f85f5b2f5074955de3c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
