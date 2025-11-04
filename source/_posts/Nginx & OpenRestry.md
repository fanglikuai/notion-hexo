---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6ZICLNY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvFlrPUuv7xVGaFpCVzqNOia%2Ft1zrWdEPcso%2FgdqO6xAIgSLmetP8E5c7KrxP9%2Ftxaq6TX9RZ2pNA0En%2FCbTAa6TYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDGqbwd3OdvtlplPxYSrcA8vPSajzN6Z5sdE6fdPpt0JhJ6XBlcpcylXy8FnvNaxJxCzOwuFpNLCdjrCx7D8g1AZapjyGYOitf09ZVUGI4dRcVTZ37cxghUZzLioRUaQTHqV7DBhmqS03ihGDR4KKdo4DJIEsu6BNiXBULcERrDM9yoCOaWdFoFhvymiWRVVYhzBWftcWQJ7X1pCrvYB7F6OBLYpGZ9EuBjcfxPFekD5vCXcF5SifRG31Aoh3rwz%2FeMAR2HT1rI%2FXNyzSBnXh1sOn0iSBcMvNH%2BciY7zczoaTxfZq8UXIr7seoy%2B5PlrQ2VgcyDV4XlaYVtpnOw9q890OzeQvoVUCUNKlfkMYMJ2A1nmkCQEo9xNQr4%2BfUiS11sUjuLaSCUI0rMKiuNNqbg2Di5XsRG%2Bf7nUuazt48HSyIzXHIDCJoysJCFy1tut4sxBTFo8wWvcs%2BDJtiiVsL96ExL%2FontwuK2eGNI873pBwkZF9Iw0thtUFfs%2Fo69q0iZvrVRmEd6jbXGSV%2FkQ7cPIkRz2HHNwAjzgdrWpUTu06zAiJXyT4CKttjrdQ2h%2Fp1%2FcVMSWLgru6UyJ4vpm9PQMLmBOeABIs7X9ZlTqceDSGaWq6ZV7slb6BjPrz7c9eQUrFWzXrIw1epLJAMMGAqcgGOqUBWSl0o9l9NSnOVAZTK2b9eI0QAndKrBKmr6YJwEDzmZMJYoxTYx2Nme%2BboMr2rcHnTFrm7ggR3k%2Fh4BvnyfZb755X5OGLcK46BKpA9D8acNtK1kl0psoSxrNR8tRRgkf8HzGD5%2B8lQ0IzXfUYAtf4KZJRD7bxE3MzN6vMmwuMsZuxnFtKXAhxUEajqkB7LVdmBBlaa1RXtLgTbHJfkd1DOs9bcLbU&X-Amz-Signature=9a940e0d7ba9c9ae012887a052ec205e64f755993d59fcbc2b87e875da536598&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
