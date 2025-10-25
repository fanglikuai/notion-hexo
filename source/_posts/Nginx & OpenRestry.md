---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPUW5HD4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBbbx41dz4DTkIneAGiVE5D9sWnhuIqHz7d5%2FGkli9QDAiEAkeHMZlPk8nbL6PQ1PtidQS6PbpDqBqp924MB57SB%2FNgq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDFhUP%2FIHgsvHxSx7JircA12TXMPLaxFzLcjPZ2tYL8pDE6UKThZmgHl%2BUFoEpPP%2BYW3Dw2GZQwdk2VnCM9BEOShmDvfA%2FnJx8mURX28uG6XQ%2Faxl9PIGc32%2FuQG%2BXgE7wAWKE5h5b0UtNayW5SZvdRQ551UdYeO4XvR8KTO9nxD1uYJOv03ts6LCPp7%2BN4XTtU%2FFu3yGaWJ7vJE3JCnzWd5yFWRxmWjRqZkEvBih%2FjMnfmW8XsHfRvmQ64osYhW%2Bfhis%2FK8JJVkDRevZI%2BLKBkqigt5%2FKIL0Fat8ABIlu2qIrkITVOXeh23nIgSk3Dj%2FyGMQjBEfDKTvy9uhAifmvXrL%2FMTpLIDSoa7prKj%2BjpQq0qWjyJF0jiJTnTeLYU0u5cGC0VRgaAb26zjup1yg38AxJWTjIza%2BHkARMmfULmJkXPLnkH%2FrkCCE1FEvb8vx7erZ4ZKVC2ZmU3TFh2cJzp1YLTH8w0kE104sBJ%2F%2Fmy1oKgJJiwPM%2F%2Bp%2Fn5YimvV%2FCG4cVnqBxtb%2Bv%2F2KDnI0nOyMSzj3LfUDp7QCxWzC1YM%2Bj%2FHBvscfT9Rw8qLOoLgv4zEHuRa9UqqGvS%2B51ClMN74jNiToc8Soozl3o0LSBKaxicrDd8TUmYl%2FaC9%2Bsu87JNVCtFfdUsrOqtPzMMyX9ccGOqUBgsoMgZ1Ts%2FJ7KiCheArkvjwhsA6bs%2B1rY%2FHTqTjmir8UWOmUxzZqHyIX2a0qmRnGQzDAXPIuwDzIRTDfRpCTEdBHnLJGgqYAso%2BXcYhaoFHBtgixvYb9%2BdDyN7J4lhaurxNeY3emQaeHzvGYKIicTu9%2BjKFXQKXYUxSKRLk5YTRYpoS0a0A2b9OpaCLlOiwgjvwrzyfpqXgFivag1jERHqEB8LIZ&X-Amz-Signature=befee34d46b7b15c8ce9fa6cc36e83c2b589ab5376267264de1459cef9d2b8d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
