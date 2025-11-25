---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IDTRRCA%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH2VQsUvQtPRKst4B7xsrHeCGqvIihupKTM8cIV2mbioAiAPFtaCeooQyy0dB8c3zDj1xLs5fOsLQBWQ8%2B7t1vgC6ir%2FAwh3EAAaDDYzNzQyMzE4MzgwNSIMmwW%2FEezX4eNp%2F398KtwDa5%2F%2FoXJzMD%2BIcPuBFRIbj8bOJXaki31aSm0w%2F6K9mUMTdYKg52eqYJZsqN9C6MhqEEB828pW4M%2BgKoBuacFNncCupzVWNyAbX2xYMi%2FOiPjl3YwqajQt6nBi3uHJBiV1tKHjOLZ%2BCLPUKRZOziAUTDRj34kxeVtFgSdvwUAfCDRFRzYLn5JuFkjhRGW%2BRZ4X4EjzkC%2BkTJNxmQiH7cbLOaM6M9fdlUS%2BlF3EpQnHkyDlhOAsIzjVUBgcY7dZ0MzJgrzV4b%2BmM3DqIeAFekvJne8qxk%2FOhSGSRrWt7NQQfB4w7jY39DjCGBAaBbSLu5kwnq2VVuU57XaGDpyPa6hYUxKrGf76ai%2BGl72u1Qoh%2FOKJr3RSKEbyD4PDrTp%2B5s7rW2nKJPWaqBoC8%2FbUC8E97Pk4md0yOBh6J5rml45%2B0hVCGDHUfsDkAISj%2B%2F0LCYsmCLxj6DASpTwN6pcTjK3NghlayG5WLQMy0ExVxZauKRoiWwaR8p%2BRXgxvTvgVderIBQSEVn0IsEcA00mc3gG2x8fSPEcS2Drs9cuSkAx%2FUjAVmBV5eqDD2VOMMbZinrDx3joG%2FZdQ9lld4Vcv7bP6bK0%2FrwCwD1AZQFiULfrsPsJZGz3dc2kjN9HmwPswh8mYyQY6pgEeu3TQkTF8NWX1nnWllqpI4i26cLdGu3eEKZW8gcKmaSP9nnDUeTkE32vjasGOZbb7w6hagxlBo%2Fj74BhJ6ka6XJlPRxCjS2%2BtohbtBwmUVgYczl4KS53oQGXYjnjho0BVprtYbmKX9y2FJzMwiQF%2F%2BJUK0CBmmPMah7bcqchWzuS1k9Y%2FIBSwlUymYYk40RLevqOPS7D2%2FILDuFvorwJO5%2B%2B5IMH5&X-Amz-Signature=1258e1f224fd897a76f45b437560e1369fb950ea9269c43ec06f8135f4efbc63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
