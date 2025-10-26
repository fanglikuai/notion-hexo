---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2S3YC4W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T070057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDclg0FEqML9QUziuMgcvmySg5lBO0%2BLY246ktoxFHgKgIhAN7joKwQs%2B333vBjUlNxcAO5LtlM%2FWJ8F%2FUmXMK2kCckKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igydo5E8Iv0cKJpuVaUq3APwKhLIMc88lSr1toO45GBHnxaKg%2F6nPNXj4upna%2B6RmPK%2BdexegnK4Xt22oHmVCIAyLcqEpMeLGpTKTunrsMFezK9oLDu3cYkyCOI0ot8XuTOcqfLO3cqg4WmOXq40ukQB6jTBa79i1fwoYLPDT%2BWQFfy4S4bknjllqUEU6LGrx9I%2FAL17ZN2300Tgjx8QRAHfLaZhz9Legx8azyzq64nJVj6IOIcHqsNzjMd1aw7dZxjpDZFmkx4QJ1k1M0UmZsdU6cVyUDrpRgR2lF0j5NqWZXKhxGfUJf%2Fs7RWaFquQ0wsz0Hm97ZAPc6OYXOhATYq%2F5mvsgludLn7RpQHyQIYJbgagy6IJhlxmRBtkVXOdwy%2FvNQYalS1wWC6PJGqibo9xoTTZIZVBc91YuF7ZTVOZxyaIyheECU7Qdh9lsZlikUo9BI0xm76E6O7kChLB4RN%2Fgs%2FdaNAT6Hp3z8c90X0oDrSgi4Scw3BquqyK1Jo74T0yvFEfTpABGTFas1w5kSeXzpE2XdwpcILjEH0lijnzLC0W8AdQByhCyoNzlih0USZTYcM9%2B%2Bl7ZtUYtDt6SWYqd79Kvs8fVBywZLbQTWzRLzBDkFTNdLsbdvVrLpOfk5XHLokiWrmLczEj8zCAgvfHBjqkAV%2Fi5xbQS1m5u95QQITs9MSOnIjqYKedTX0q5l5YW7xdRw%2Ffi2sAxWf0pBXl7MpT56yzqz9rj3axQSYLAXFoM9XWcHCtzkH6N1Nh7rsOnSFYTOKwaOuNlCMWbIrzeUGwY4QFSDaUbw1xRdIUQ3RlVONi1PhNz7QSSZRoRncMYrd7vR0HBpIUcN4ocVkIE9w9b9KmAXpJCzv87d4m118CfMU9JZ8l&X-Amz-Signature=0eeca05fcc527a91012ec4dedbde24367a592f7c95d97a893cc0c78057b2dc83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
