---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AIK2T6N%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIQCBpqazD5J5Hh9y2ftccOcjcegUHXxoLOOyQMCZz4Xw6gIgLJwHkFtxuntXkFbStrW6d%2FShESZZLtV9atINjAfjkmIq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDIPbMEb9E9UKG80Y0CrcA%2BqWAnZKCF5vCiC%2FisJnEueMN8Yxe6YvEiSj1Z6sDVmtbxArsPjwvvjT%2B7qJX2OOzb%2FklwozDtgn0qr9lf8ONlXONYjd%2FWt6iRn5GEv8VQS%2BGKfwCf4kkYnY3YtzuzzJjUSmboCyy1xFt%2FiZ88lc2WwY8Nfz1Ywaq4Y8oiNaY%2BZcKV5QXuB2fgidYVZs4dpQ1scjietk8PsVYLqCl99m4JLp7ncI629j1sO4DPDmpLLI7iyX2n854rBCcEzYzvbOfVOgTx7iy%2B0IBfSvqivYBmPHzWKlUl%2F%2F2BKq8Lrt0Pr%2BZHhUwgxv92lPmyIJuJIduU2TFMyvXPgj6XVf22ltVY5y%2F0c%2F6KBmogRAaXnerhtyF7Am3egA2fDAgY6zhpp8YiNZbmiVajtxc5s%2BDzsYJdjk6zlmCM9EA4r1S%2BZ%2BAuSBVnAZRMn9WJ1dNVr6HsLe0UwirrX%2FKEl43vsaJz5r0RAJjND0kBCGp8ZlHYi%2F2mZE5Bl8o5JnCgysZcMmjnLvD60IcLVxEhqAkIz5qhuSwkscggzpxJIq1zifWI5xtvEamcO8VYU1LSvBpvDM2%2F3ZNNAQnREY5boN6g%2B70MAaqGAWyQMvbuHoiRzZlASpzDKDy%2BjQaZBDFzBqvJ%2BGMOuUm8gGOqUB82JCyCRLYxyTA3D1Mfszl86qfwblr5itQDg4R3OuDjpiqh4g%2BFLSutAbjofkqdxYwBpCVBU2Oc0OxWFhQvun6aplCc63aDJfBy4tbyYnrO7wudEXFmoi2AmzYDjsQKbVBs%2Fs126kG45D%2FVphWU8PWTKyEvVvtZs1gE4bfJeP%2Bqh7fdaweuk8FQJmlBoVJnSTna0x%2Bvd5xTkBJxiAy5%2FQFt5%2FShMr&X-Amz-Signature=005a2f8be86c5e52f1f25d1676692f797b4bf4836e90819a4d7b951bf0eed3a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
