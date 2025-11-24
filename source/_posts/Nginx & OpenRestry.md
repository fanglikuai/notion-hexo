---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IOYNLGR%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T150158Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjE8U29xiu7mfofam%2Bpnx9G2NdV7VBapyZQUd3zgggSQIhAPy4wKCqt9wjS%2BgZgzpCvBWkQcDCrGZvuQRX1gdtDOy2Kv8DCFgQABoMNjM3NDIzMTgzODA1Igx0fQkhRQa0W7TEzpEq3AMC5znRTxAmttIh8Q%2BX57d8GqjCoKw1GvFhKUFRzkad70NjsiPCzfGtBWz8OG5ZIAQnRNvklpxTEIP13UgOdQJTxmqK1hX3yYRkB9dFrZm4aJ3xfTEotuK4BMuwAZaHek6D248%2FVIKcHc12drDW3EskxzWUFECsQh8gh%2F9ZEoqdQpbD5Dyhtle5A65yDIgOod65t3WfiO9B3Zr8hiqiJUNC8Io%2F8341LIFZ7GOyszEtz2VaWzLobkmuUqxvN3dSzCZfgprrnrmhycD%2B53Owq8VKzmWfK1CTPqIKOqVXLPTkZjsiG5oakBhm8zYB7QT3EUpJHDtKHZ%2F4aPp2l0QNK4XYxkbRdUnM4HxS91t%2FEZorU6T7Dd6vSwerMupOTD7OfCqhDKXES9GRnpEHmRz6tpwlJ1ZQXYf6opiAgd6LaaiOu2G09OiI946HfPCJEGMExSJvAeTTIpyNublASVTqFLY2UYOyLXXiVvlPYovxk4HVfUArqXMiWXululRp47zD9QybhmWwYRLX7G0wS59EHhLV0vAo7HnIgOCXTH%2BKvDdN31PtsHCkwWC2hzOYkIQQ%2F80TXl17iTprIoBkRj8XD9tY%2FCk6uUXdnzylPxgNw9kC0%2FVhcbbvX3qeqL01iTCa2pHJBjqkAVQrkFX7n5XBezHMweEDlJ4W0QQjY1uT98eZdCTuLO3cpfRTtmm3MVbumPfnzQhveCX50tpM8Ajpdt3gln7MBI84vOpgTDwZ8dbvIyXxdkqe7RWc0t59vAxJQ6qoyBUTL6a2izeQ3E43bARbl5Txzno%2FlG91b0QnfVdmijn5vqAui5D3cowHTpkgQIzuPZ8vnTWabAL2MaVK1mefD8ZQijkQG9a0&X-Amz-Signature=166b2cafae18d73b640781905ffa29048922dda57d38f7045196a6950d88233a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
