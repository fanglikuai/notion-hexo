---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7JJKXOB%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIChwcraQVZ0ojuyvhSRwEiEoAdzMH4dBdy9P95J%2FmtN1AiEAsrLVHOYxFUid2m2r4deBydHTri4QsxQtOy%2B0rKeuSuYqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDClW9bVLGD6OSSh4WCrcA7wWFuW%2Bye448ODb2nFLqE%2FYoz%2Fs6x9IOvQVmuNeaOI%2F%2BDmE3oizXfGt5sycloXyCNBki6Ow5EfQhIdUZyEb4RGpz63KwljDHDc5jFiVsYT70G%2BEacQ3%2Bov5PQegWOKi5%2B8XXdVNrBSdCqZxFS5SzpwMnY0F4X1E5WWwktRUCJPvYBrq5b8XnwPLjXJVTiPFEIUc4fa1sZNJ42R0TLFXMj5RTVWwas4f3ijDGKWNWkqLUHUipnakpaHroyGwlCv89WcPGa52uBCA0hxQiNrClNQk5tNH9V1Viz%2BYp848LZ8oJHqKXFw3pRypuRsohYbRE6BPn3F2NaOr97V%2BdSZ%2BRcFm3W87WKxVBpuz%2BpnPmRkwOT0Y2G4mpe2QVhLm9MW1RCUlN4zllkse1%2FeEKyFDYtRHAEqDLrsuuv5lazaihhPG2Ylj5DmEmNWiJTH7RmAv3LgAUkVuq9gBiz3vAyTPDGgLOa1PStXizIBSYdMROYNwKoVxrFXyNkonLf25iNjqbpnKpCKiKbdap1O2Qshe7dVql8q9WDMqZjJa4q8cQIpQ3pipoHed8zrJttLhpayt%2BLVJIUWHJ8N7NiIB%2BRa304990Lw2usFw6y7HfMZudCiDp4LhKDhEx4pnMLePML35sMYGOqUBxE27i7lcSItPPnbV02YJdrC6UBzbMKvvR9SWKUEKonoPC1H%2FoapGKCbXW3iXkWwq6Fj6qum0btfqtF8mSvipHorxiDvxWKei2rWFZqRa1Y32nGhB6fNLDXPOMyOQsIs0zq4DpXBM6kL7J4D89lYRYhXV%2BUT%2BbIPmWzfInzhNNOxjJpkcM%2B07nZrhy1SUOsb22T1h%2FabwefboLWkEQ2ZmJQjhXzkQ&X-Amz-Signature=43e99a28552efd16f9f3eb91e2c33a70678c9c8bfa800bb602592d7e8f8c38e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
