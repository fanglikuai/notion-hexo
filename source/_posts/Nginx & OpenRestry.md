---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WXQW7MI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T160110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGnSbvBMxsZTk2NHSVLzjmqWq9Y8jvBMUQJhOcxXxEZdAiASx54lX5PTyaKmnNe2sJWKtF26MA7VQ%2BOqHvsmRas%2F7CqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIvJQlyKFHdCHme1EKtwDG5Z0MhttGPMfCoIKBRb4iKfPE9MSteQq4jSkmt%2FvtvEIh3GylW4KvKQcOsUnco4afE8vLywhdg10NTY11K48siRweN7DS%2BhhhNuZob%2F3GrQNEwYlcMo4j1aJmhswqMrwsonkXjL4bZO9kETcbfvJ5sSLiKdqx%2BgV0KtvPkVIGsflhJdk3htfK2pDcTsnqUhcTt%2BwxxkXT0F7TaDKTCMzKRK3ACQ3PMkITDqil5jixLKRpzyni6z1wB6tsh97LjR3I1OQ0VoXDP7qfuIMGU%2BnyqfgEeRiUqP2kVNi4mYxkU%2F3uSuvl8ukd68vTp1G4iAFHZykGpVtXi90MvwY3hfbB7e1sxsKf82xcegXZNfaFkfDmAUhBJaCvR3HW2oDrH8Z2Ds66UvKp2DAlT5xXms9YtBOnAzSK6e%2BKwhNCfCGsW8USQqjOxew9Qzd%2BUvT8u%2Bz%2Fon7Ab7f6fa2yGtgQyJMNooql2Zs%2Bb0JGB2ITSata%2B6WYk3HKlHhgjMOICNTp3ix6L2zLn1PPzPv%2FWFddOzcD8QB45TTcnpYJOsoRQqU0YgkBC2RTkEwgQ5SoruXK9Qb7AhXU0%2Fi1Pu0%2F5dckZHZ5n%2BelVhdMgOkIIQQJBidaZuSWs%2Bse9F3LpnzaCow87K9yAY6pgG9VXEslSKwrGzvi3gfQVPZAupNo9AMuVFNUqTntsnX454C6cJ2FPZuAa0xARjEvrusA3%2Bsz4JIyrhrUc0TXAZ3lfGtWaLO81BQz5yQ8JHSXVV55z9IQAnuNAiNMLfYSLPyqCTfKxNUh04PIrepzCkk3M2TnmkDdpNE1EdDDoohbk0wxUGMi6GtZBdOzwcX%2FLoEOx%2Fl8YzH9BDYWaGsJz4OuCRfw0T6&X-Amz-Signature=b2db735a280cff07f65f9793dc50757d59a9a6bb5e5d954cc68391ba5949d3a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
