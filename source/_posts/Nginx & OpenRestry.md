---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7CGMXL4%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCACM0Brxlt7fng%2Fn185Oroxhr%2B%2FBlDjzvlt36w7FxBFQIhAOy0VwPx3JZ3EMp8VVzLcwFF8KvCGm3mwcDfnYsKryLnKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxYT6guqVF7hupMP1Iq3ANiHebzvkrYzDC%2FBw9RQLH4nN4t1mr6M7PDPD5eCQQz2iJaQbFdQO3dO2RDzYKzyKHXgOxABzAU0vyIggk771yoFpdshUm2L%2FfZxfo%2B1wBJprwFSYsnK1YEahdpnyFO2LaKSEkAWATHReP2%2FhiwsUgxk6bk9aXidRty3KTC4si%2B242tjAT9QEAIn%2FxKU2c4liJHHgZPtHY%2BxQyf2WGju7UZKvYBq1FAXEXV99Qr%2Bi%2F4wcoYYQhucLToLeVXp2GEFoGqfbMmHUgfP7GI08WVML%2F0SqWQ0WLiKShff0RZpq3RbPWFkre%2BKnUcEDcUO8pzlGu%2FAa%2BKdQgcc54FqkqtfW%2B1mk7Tx1LG9UoxJl8%2BWq4dtSQSF1cGvkjoWY8SXuuxdFHVMifKIO7Zs4iT%2FaJBJiv3cEqjhJYTTQHjdUQsUd7Sp5cCI4Vy4MyvM4pjC984Opz5KMK%2FJ3%2BFrdgeW%2ByRmN4lJKdLXCIQNRIIzulPvlQKg0s54OaxkOV4e0yVR2LUENNcsIF4AaH5JlwW5b%2B2X%2BRwqg3GJbsJdtEF9QwFLnkUmoOWpyrsj%2BhUGdDxND3zB3tPnYAqeUOXzr%2BQrcj3i9%2BSDMd%2FjXDM%2FpzudGy2GgU6xoCLi8BCLW4ByvIEhzC34t7GBjqkAU96sHdtrK6U19O4TP9cy8%2BpRxxKO51jipnbDg1JD00eB4ZoBLTp55GXQnuvpaD9c%2FCp9R8drbqoQcnx7dtCSDG5BhJyDQ%2BnNuGKmDW5fdJ67aLzkj07wJN5QFeeDbNKOZ%2FH602fKLouIVtVK%2BpyczzEICXEhwKjvA9pM1gG5FQlBqw2mX8IqSZ7jOWADYfAmgKqiEiYlwWcnNbtJfzrVMs7Im8e&X-Amz-Signature=b8e8e79291371672032a3cb56d57cab5a55c30253f2f620bf4d90a82e6321b7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
