---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCVEHHJ3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVupvX47xv7A4x9zkHTDi%2FRkJgc9bI2nV2IgnIVUVp%2BAiEArWXRJLkRNz3lTgQOQpaTHHNAW3J%2B4w%2BQfixqfKXnzNMq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDMRF2%2Bxp7O4yljYgiyrcAzFHo1u9hc1MDmf9B8TBHxiHmAX0oTF2aGfSRHf8R7oZPub0swVliuv5ioCdAhrP%2Bz8cl7OiyT2hY%2B%2FRs3Fx1AMpbNhEl8SMoLIISBAAhLek4gfnRhzDi3vgHdj3scFRLKG8w7fXBfUpQKoJG4V2nUxRVvBFWFkfawiLVuqKHsRN3YHLdmXTN9vbxIB93TQHiJYit4tCtuw8q0mPVcFM35cPC7sUIiJRkPTTCoWa7s3NrLHZb8OpgGH4jom2FFt0NJFBwAdsFNj8yzF1lA2APpGEsd7tY%2FoxD93mTzMEHdUhNHC87ulwABDRaP8uE4pGqRO8rrOfxPB3dKeiocN3pTZxiJC6yoGx31KJ9%2FNtQc%2BUpEHOCwEGy%2BlhdE459sOWQPHpOdqZ7tI6hFP%2FJqGTQ%2FAyGXxDy7%2Fr6dySOaNPzU7Y7ecd%2FPxuBtIqxpZCYUyHzAYO8blxwtcSIAtnNa5PIX6lka1uLzPOq5532om5YiCkphKwKDBNV%2FMJw6xUIMInGMVW4YULHjpvHkbbuuS8mMXgPrkq4cmQK6km0mQ7ash6iH%2F6%2BQUdzwA4mtNLTXGdBkfuVpFgV78THKgVPmuJYAcLIi7UlD2j2sISnBJX%2FktGeeRx11PAlu1IsSrBMMeH08YGOqUB941VmpyoolnihT2SWFnWxNzOLy4yUfHd2o%2BnrjFRl%2F6QJ9YAm6EGDClb5tXOIKkxhlQiN5sM7r8uIqBnidaKWZXez5JbtR70FXGp%2Fh1Tyx9z4TGZlrzVt5viHADH6DWGY7jXxEcudegs50M3iRrEfisDriy%2BcXMCmnXpjqn6sGX4Rsa3X3EOwwUCuZdj4Gm6rmlKFeHKOZg20aUro%2BMxpfRJAqdr&X-Amz-Signature=cfdf849abd4a7bf0c03415d5d271b320323c7d10103eca44392196ce93846972&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
