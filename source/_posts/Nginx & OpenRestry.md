---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MEB2Q5H%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJIMEYCIQDFTJyJSN7wliUT2fUqNL7zYrV5IyNs5AzZ8hJjiNL1hwIhAMffrRdvitLmjQp869Mnmtxs6KyxnqijeafO000bS45nKv8DCBcQABoMNjM3NDIzMTgzODA1Igz4B%2B1%2BLnzJPYhqNKYq3ANmcBFAv%2FpGSjWS0M%2F5%2Bo8qPZrJOexq9MOtL0zD9G5MMDR9FopHscFgTyYogeH%2Bpo7Mq5P9CUe0P6K%2Fqf%2BCf8gxYREWVJqppo8LbV4I3hYwhDZKRqVg00KS7swyOkhHR34nmx2dOhjPP8tCRWEerxAQNsGkB%2BeP9%2FliMrSl8M3%2FdM8nVVH1K7M1kjor%2BRWP5qbU%2Fk0LhDwXNoeAZ7zOoTe%2Bni66PVLgbRFTXbp65u83YmZFemR7qFzxjg8saB%2BBMtizguYdfP%2FBIml2d2Du8lf7tPJSUAnowOXfj8Jlmdqge2uoUw01DcPXoU561ECCGEOVKoRo7IRUgfqh4pvDgDopDYQitgIfk8jTP2L8G6HyDrZzoQSEQhJUlp847lmMRfIzpUIDitTwohFHtg0IjgMgQnM9j%2BWqu1t78ZFq35No4kgRt9e9TUWtXgg7C96nXNdNIm6U2NMd0ztuxYKZpg4jfR48VHQsGo9Z77lgg1tqoAY6BRduRgzRM865xcxh7eK6s3zpdYquBLhTJkg1Rf6bQ1cMsmXHEQuUGExiQemkDezvgxvb4s9x0bV%2BDgDWbvnvC8UXb5kj0RzuzCyivTkDWFL6wamEZz8IGtjDzPxlZwRYtL%2FnnV%2FUYFdEVTDBpcvIBjqkAeN0be19ZCLK4POSMC1q7tBLK5oU191CF8EvyNcBmPgzkbhRSB6d%2BCJnrcuvh7cqtCgx2%2FmB71EYlCc1dHOxe%2BoACHUqA6GhhzzGpykuNtuneAXS2wIBhUOoDHKmn8pw5PZ1HTHtu3vNYjuTHGQ%2Bh7YYNsCqH6h0%2BOZFeMUm1EWfxYInVUDlgc1aGFCA5T%2FI4EoWu4A56OyXBWokpYAqza6P90%2F%2F&X-Amz-Signature=d23128bd9bd3cfdba1177eb1c709788d5c867361a5e56185a96b6f61f9b0d736&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
