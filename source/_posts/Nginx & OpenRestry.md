---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ETG4EPW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAXg55j9PbKADkjwz4t4zuxscn2%2FALLJQ6TLK6RJgCwAiB4AwNO3Tlx9zSyo8%2Ba8uNjPN9me2I2cJqK4FU0k4t07CqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMukNMZxEEK5lJsMFrKtwDjg%2FUAHcJuF%2FSA1RJHc4g0VsCfyCEyQUsgW1yvEyd1oKACAdF1kG6EsH%2FHBLxguc4fg2fje46oR6m8pxnz6vu1jo2p8WUs6pQW1MqkAJWo7OlZZ8J2vXu8Aefyj55cqLatFbD9%2BJR5Pr28htUfCrIQOnz6zHU2bO7x0sALYUocK24jDRro6Li6cja9sXQkASEmJsN%2BWINrNufovlqrn%2FWmrhU5K%2Fx7%2F0PMqfvvrOOeP1vddRlFzZJyxc7Sz%2FDMYdEIE%2Fh0r9EA%2FDZtN8HIDc1%2F5yT%2BbUTRZC6z3ipkf5ft0jTiZHaj5jUIPO8LSTK2va%2BJQgY2jFYu9SIw9jVamBDbBK73bJ1N6q0snIuCsDypXBCpKl5McVJLj4%2FqzO969F%2BXqh%2Fe%2BUqXu6JDaSry9819u4a8ds%2Fvn5DcfXA421saaaug8yjKoX7a1VxkJHRnCi6rwZb5%2FqdtQMYMQPVWvX%2FYgo2GRmBoI3Zn%2Fod6PESMGZhoY0IEAXs5y4oe7obOq4pTrHkPUVDsV%2FxkowAm56y4%2FPyhKWVVweWqXrhKQy6OHtygj82pnsIFOjTj%2FesGwesNjbw4VmOjlRhNgwah8irDYY%2BZmlu3AnwAFmzc4eT6R5McJO3UZK8HvkL4dAwi8muyAY6pgE%2BntSoPieDR6%2BSP8GDRTqdHcLLHsB8wozX%2Bv3LwCAhNVz%2BpbCfVVxFNKyLHWR7T1sULR%2F8hDsfidk27SmBxP5wWb8B%2BlyKO7oJz8yu0noaeMfPymLRo7TbjSSngXtL6a3bIHGTgIUadeI0J1uId3T1iy5HcLui%2F4pk1htgszXTYSywHZj%2BCLQtHOlu7JJXAj8%2FJghtv7NwJsqP6Yj8D%2FgwpKGuZLv2&X-Amz-Signature=3dc1bfdb0333c189028cf5fa1924aad562a4be2c2b7d0f06434f44e86b6100ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
