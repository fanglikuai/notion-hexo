---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRNRQ43U%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCr7ts4Koj3adkfutEQRiA4kT2nPoiu%2BXIqm4d44idW2wIgap4LMFoLC2nNkAqdZbyZl1tkiiv17Jr1SjOH835hhT0q%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDGJWwnXziZw6oT3CVCrcA%2FZJk0MIMQa%2B85ocaxi3nSc2JtWlcOLt3NdjpMUGYg8qDtpq5AENxgYAj1JTidT0VOeomwI2fbVleeE%2BjJZR2g6TI6tVFmiOAcj%2FXV4IS6xkVZy6dScUuWQpx9PHSccQhJNZUes0AaHRXdMQ659xmQ%2BwEnFXOwCwNkxXXeuLCVY0%2BfBkpr9bTiLLMPEh%2F2GEJucWOkRarZyRF5Cz4reXiqqlbweQWS3DmKIFdfffiSaQamTvh%2FQpKwZB8WFNz09QVRSnpiOCwWKGwG99%2BCfHZNaLOm8hK%2FkPeo0LlepQTcBOyChjh7TcN4KAN%2BBSJzMHzp0YIdoGdRunfPN3yqtzEf%2B%2FmYdMczyWEm63fAjpYbFbhEavl3ccDAZIlBSbTebWu4oCJ27mJ%2BPZBqkHV1aZf83J6bxrMLcLNt21A96EKcqIKrl3UK9Uwz1IAtIkErzyDi2OQBQU59AE9mtAW5nAAYoHGxjYKsUPVJq9XTHcFjKdz0HDIvAkPmOYG%2FZjBuGF%2FcDaPAlvQHtPkiw9kM3f7qPXBr6x54RZYJXtc2AfTvmOQRwdTOAj3XP7Z%2B5M6WBfzJ2ekc53ikSQSj0kEUX8btX1ACCHf4dsXXxmFWcOiMElcQPO5NhBN%2Fz7Q4WjML3LyMgGOqUBGHk%2FJzCU5pQ7zeUaCZhc3ULykK%2BFvWXWfHInD7khanoN6pP%2FaNjvOaCaNZlVqfkZo6FO%2BQ6jTcz9redHobuLYq4nM4ygHtansY7f%2Fk44MjnDhEXXOSoZbWe2ZLO6Kni64S9wv0aBuJ293V0ML%2Ff%2Fwjjvn6FEmSdJErPBRIicGr1Xpek3lytSTNf1GCTxw6b9npweDwcZF7fKW6HWoTABJI5caFS6&X-Amz-Signature=dca54b3c2c1bfbf43e5cddf7f490e2000ed6c59742126b23531704b5434e9ae4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
