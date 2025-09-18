---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662TWRCK2G%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIEIXXNu0KACcBFq72nIHzNAKCsihD6PBTzEmZZzaLFhxAiAw5ACcs6qfdzTEyyj2Owtq6akxktwUTqqGTopm81w0bCqIBAix%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZJuoUs0gDNdtVwooKtwDwiVYxKoMaJqZS%2BZohTHgP2IGXV7Kt3jdNoXYk7YESJdZpIpmZ3hqwQU4AGQLDLx5%2B5qAJlOC2mdO5aNsqX00uBDj23YbJbo63%2BtzW3kEbFI8a9LOJQ7llfBYX4TTabuH4rH%2BXfJYsTFqWIinaCspJ%2FBCaDn86rLj%2FloYZzem06Foy36un7O37P%2BsAIyH8Fm1646bs7tHs7zPgnBSvrz9%2BApi37eHAuYprOUpotygB78%2Bp7MJ%2FwLz%2FCU9wY%2FJgyKxkf5TC6ciRDwnMSFBFJ6pd%2Fp30gAtMLAc0KWqgyChewTXMXvz8%2Bxd5m4%2FK%2BFOYvwWckYNgiEDCQyZOhjm6%2FczJtbwE1M4UluT8T5uGlj4v4umuudOyzYTUIiWrp8SNSEtrv3bKfctgkhIAaicT0kbe6jo3z202%2B0%2F2UhlwAigGY7TyeMawN9lGlXx4LpGh7PN%2F%2F3ExsPN43F6lafSiv%2B2JTMXsAsOh81dLf0yoO%2BBRrhCVmjM5bV1lO44X1sPzQDHJt7T%2B9UyIsPqjm5X56QI5qJacAHs1pLd53vIuihmPYv1zOgkeUVCVYUJ4A5Fkx3BtN%2FVwbSj7n%2Bs3o3xqSK9Gh23ENSmZGrfo4wKBCfyfv3etST9qkiUwHzF1Bgw95mtxgY6pgEQg2zcaNFGvHw8RVKBOqQGIEW2n0SDr2XH0aXQ2DwmyZMfnhSvXMxuyUTXi%2FalhbPoxZKtEaFAdIjaD42juDjOXY6D%2BTdHGfIRZZJ%2Bh%2FYmAaUAzzxLUcM%2BHNpM8%2F9WkJ20ntMDTOgWL9jd00uPJxuGreOQJZ4LqMweXzAp%2Fh0bRO6Wcx19Odpd0UwCaZuQUKEBP4T6pVfsZnERrSFe04RR14iL92DD&X-Amz-Signature=6a68e5b5b716ac0f4f315f249fac79c3ca930eaa004a57967a4c2e023d1a77d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
