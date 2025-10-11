---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNVB4ZH6%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDJUVWohvOi0DjeNHoOZcUIK5PsjN%2FQXoWp8ZuOY8ZMOQIhAJSssSlF%2FCHBIvXhgjGCvPiWCyZRsLGhH5dU7A61s6pcKv8DCBYQABoMNjM3NDIzMTgzODA1IgxTK0dfcxariM1ckBIq3AMcgZd4hTFuklGUt1lw7EXXn5FZi8VPYzOwqhGrEe64M6QVypfTIwm3%2FVVgRGzSLKR8vrFIreSQ2UKfJdWBqpGvG%2BQvL7FQZWK48709RuRiU1OBQnTVbQdJNfPBJOvUQGQwZjt16ce4LX6bFKA4C7bnxfnkL%2FvmBdTqINheExuccNWREE22DPyoKO%2FcKdHmsCpnUpm8wh%2B%2BaoWjrZQyc7R3mh9b3T%2FAa6onHFlwzOIE2WEvUMH6W%2BPwODoaTCVl0ZP%2BePK7TYdwWkCtO0Ih9jkNdnzAMiagnjCOTlHsZ7K3wFIit8OtrvGDu%2BClUvotxtQ%2F7pHkYEv%2B7aZj2hblC3i7kpcJaxRD4JlQFL2%2BGXnySG4qGj3vMELMnBpGc3gd7fcy2o4veDFlR1PNNHjAAPwwWeuzecK%2FcdGUlg6XNvMqb7jdzu%2FF2KH0DDcwMZ79mf7jDvYVKgLrQROEgS0smOKJ0twH0p%2FY28ftgD1GSqHRY1l4%2FRmxS%2FSb2jvHAcwDJUqm1yn0oFlPlzxfPpE6rldqlI3b%2FAebnY9FNdwCbIupwqN07GTnc3BV%2FnJlf7T3j0%2F1D9Oc0K5vxqJ6m83xz0fUJ1At16N2ZToMoWokoSD9uvVVX3BuXBYdfZ7KhTCOpKnHBjqkAZWiOXvfE5OyMboTpzo9zuJqDivAHsJQFm3ugJtGrfsm%2BViCkB7QcSMdlZhcojSPZYZ5CTm%2FqJWwwRDtKTzInnK4QgpaL5BxIvNywi2jpK4TXixdYFGWVK5AkJ6SmAQh5cpAlFnc515Nt4RvnhzNlHGT1elL8wNVUXBAVcD8%2FF5%2FJhnZVEj%2FFPyM42wGrFuWJFOtYDrwLQTwa73QlZgzTMfTLTBw&X-Amz-Signature=77516643965ab65674167c2b716bb4b7416ba2ddc816ba0923721f882bc46bcb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
