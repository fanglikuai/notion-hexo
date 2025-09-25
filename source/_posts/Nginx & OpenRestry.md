---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQQMJD72%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEkH%2FAddtrFmPqZnh%2FPTosW9t7b0yvQpEoYYSJRy5A6zAiAF7nkOvakAfTTonar4XBUox%2BQPvn23v8v8MoiV150xRir%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMDIg824oPSe%2FkjDqdKtwDYuwNHiTwVUBqegDzRStaeWyxjVkfh4HggBC7OboqLROsyCHKLKcK%2FXT56oHh3sJcc4oFkVmWgNiF7YyL9KFYDVTPowl0tHvzVvuKTHXR2nrV%2Fs3pZ1K%2FVpjiCxejJEEpcfxexuIeN2psD9Rh5OOVx%2FyaUWYMd2OnKHi65dWNYkqCxWGbgwcaxqxFyi%2BTCVujpghcPGBiKuDiCBaNf4IZNSdReHxsQEqviwITubHiNTPpF41%2BsXanrIiMC6jtjpWzWOUY4K1zd8FKwN9RTlsZU116PpMThiv0AhRbK4u%2F2C9LOxu4XamFtClo1odVq0ripvkpXqtap%2BjgyvQ7mbi%2BL5xLxg1ZrBAHYUVPzOpMSg8ebCWimfwYXsoiItcAbrKad48bvRWRtosJ7TsPYtgKgNAZmE7V57JlDsOhdgx2hgfiTlTyyECHKWlnxn5r5bPLdbbKMqBbjO5dpeo7RiAiqYrul75vbwR0JiBF4ySP4aIZMdDnA35oYlcUgwgugxZjEk3fd7zTBcw9AePL75Cmdmu8AwXnRkbssd1niUDpDHPrzAaamB6%2FwUHy%2FKV%2FPOGoTQOVbpovxDhYymQijyowbmqbQ9%2FJHf6o8CA%2F81G%2B9moJr3ac4toVCmwo5ekwtfLTxgY6pgEBuy%2Fo%2BK53tj5hNeqrUhOMkXMXHfdJKhlKs%2FA2GyygAVM5FUPfsRqVkmJpDZ6maeTszRz9eQvIE%2BGJCcoOLZz7R3S3VMX1p8KZ9lmbKoa8Il8m3ykWaikjtSVbp6PFoOQYL8Hd8QKb%2FtZVjmvoruqBeneHiE%2FcH7vs8Wj9aS%2Fn%2F2fEQlbs%2BwobTM1y4XQZquo0XELlwGzy8y3IJSeoTpxvq6JC4Hj3&X-Amz-Signature=06fbd59ed47c793d0898bc79979e7af2bba9d16e97f605c4744dab04e1210f57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
