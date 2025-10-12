---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WL5H3DXQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA%2FhXD5hdse0RjU7EjmrxRyezh6RDOTteTbbLzw5Va%2BiAiA9tGCW97zPF2LnBV%2FCFtRZUkr5Ki%2FEc%2BMZoKUGZQQ%2FKSr%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMM%2BXhtn6EwrDD%2BnY0KtwDTfHFb%2Fur6CbRWJRYkyuBdowrPCYD5jRagCDfChhjyEcvsISMwpdXnRzgrOp1f%2FYyJdJvpnULdoa7TXu5OKjDf3DWgoY1%2BqLwSoYjf2V%2FmHZSU6JvUjG7Np6jtwEDORomNU35qjLvxZhz05PXhwromEVZoIVhQeOBk0Cc0Ndi452%2FDH%2FD%2Bk0UYprDFLklrEJnrIYSOgnq8eitvbA385vmM%2FRExEuDOfmvmSscKMesTo9IAQn4y3lHEPQap18Dez5cl2iF2ypxIWmGjDG020Jv8696ueivMRiAYDkIjy5ywqYc3vRsNKxUSm%2FUokiTnkAGJFHib%2FhIHptBAVOLN0FLzLXogG32JY%2BdoMZTVYM%2Fb%2FnOsR1eEGtIrxOgkne98K6oNnNmrLMvJFYVeo6WP25LyOa5BBEEEKnSrMP6G7H5ogXALJEZiRhoEgqfAS3mH61yl1%2BZ3VCpMHFCnQnHcLgsc%2Fm51nF0XWCGc6rUsQYw%2BQPGTh2DMJOEJLu4UrT%2FSwToNEBx3PKO%2FRDDegv4vyIbD%2By4DM%2FXPK70wyYxKrNlJV4sWgk84AGh33xiYSwZfnhEyag%2B8XZ73%2F6zUjqJCxEFigs9MGxTr7zw%2Fi61yA2PraPTjI%2BTJ6fSD1GD6OQwneWtxwY6pgHOFi2IzJOpxEo4ftGNG0FIgEOQJhOjc%2BDCq6YZcKFMEiL3%2BlNgmPMQk7yCpKaMKQs3O%2Btj80zHGgrCQnFGohgGk3fKSiqLbpUddETuz4wZK%2FoIpUSPD0Q05FXFknA93whWLcfA8mAvsNHCfAtP0gKk5PRdF6dscsfdK5Kax5vp8fcoawjX7MNLpvJ1t3y1x2AyYJbZFsMcxMq5mGvhwKiUMd%2FOuJ7v&X-Amz-Signature=b1073f7f18b85cf6961531d7608189b81d717bdb4546adb081e603287a69e02c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
