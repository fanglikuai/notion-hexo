---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRSLPEE5%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJGMEQCIA4GKiGcrBW%2B2mXLrVq3SLY2cEjRF%2FC1%2F9xvznFunyyuAiBRu4s9vB0ETCdt8w97dzNb1MEGeVITVSsBNDA9i2N5CyqIBAi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXiR66kXR9Ld1pxz3KtwDGMAHKkinEYBpiZnadwoEFRurHSpENBIyxK9EUFr3BKISHEd3THaiYZSjGYLsesA7wW6%2FwoEI3qL8ceL7nsCeFVbrDKXymuOx%2B9mWOgUBBiWLJTjx3o3teN2cnSLW8aPepbXFc4F2EbfLDmJxoWz62Jq%2Fu0C9xr7hxLOswkLGmKOsQO8XWK2YjwEhQ6NBMni4w4TJw5pNcsuUR4nM%2BDln2ZVFH7w5uqD7mVA9ic6LL5At7TNmXKjZQ60JFdnGrKs%2BaOYvzdGnxCltHXrWkX5ESgZLDbFGQEWIDy0a9DWrvlLCyQwjzGhE%2BHmqSPW5IjqQXnbKd5nYCOkdJ19wMzMmI%2BaIcUO%2FBkdNS3Z%2B7auIsboUeqTVB0NTz4AykV69mZrINlWa56xffPMARN3BQYxMWOmaS5p73dEeqYY4yTOrhjExNsjdB74fl6zrTvcEsqzzdEBnrkIUc8FiN3FRNgC4Wlmscqs%2BQn7I3%2B4arX%2FWBDHD2oGMLUgZGxyWv%2FhoTGcH3gaJ0ugqC3cK0547yAtB1bKnsNL%2FxZk5EdMQkupcoFekdM5KxF5%2FLuwBWLSI6rXnegQHSM79OS%2B0su%2BYaa7DPN7kKKEuCHTtGVQcktk%2F1mmZqpQcIL4ZSy2irKIwv%2B%2FkxgY6pgH80Ius6RhiX1MbIsJq801dZO4ROitn2edOZ%2F57u55jmYxYtwXPMmDiQF4Snas%2BiL3fvwtsGfFyMzy1symZP4%2BmBh84lHwwTtQNiERcO6e%2BmYZXouHOcCYaRAEEP0RSZzOXZZHI2ISReSupMJwSETlGjhVP%2FLFAEGFxsJGIWq10U8TW5yXZUtwbBfYZS%2FIP32xmlooMRq10mjQIsDg3oHfZ5nWZXgPy&X-Amz-Signature=d1a765016700407e332bdee26bbbab0d92bd7a7e1b972ed1185ee22d62d3f0f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
