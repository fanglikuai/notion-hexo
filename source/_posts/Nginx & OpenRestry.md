---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFIU7O6J%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T150056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBpyt4u46YyAiMkHBqqhOawA%2FevFO9m0KRk1mqrEe46%2FAiEAhlJKjPAt7p3txKfKAVWdo99hgzQKKYFqGaVbrf1ocrAqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBTYw5qtdZsw8G3EYSrcA2KRBZsnxoiWmN1I9g5K67mZOaMSCl26Ks4WbKGLlqNbpz8rJyYf7F9BemHoJFmeaP0oZr4%2F%2BxGdF8084B5m80Q11QFe7Di0uGc0OwOgPBsW4a0dqFQCI22nfn7Vz3PjusZj4ObaN2ShG8YGmrgAoIhTrTTSNIXC7I6l7xeHTVuJCQfvPWl12ornyB%2FtGCWg9pOgQWVDvwoGC315Geh54FeH1hsGpkqSlfi0s1Y%2F1okcKCbPBBqimesIn93mJhUbUlpBNo0lMvfSJb7M9B3HhKZy0rBPaHk8Lf%2BqdWvf5ozSCTpwknvfk72oUq%2FNjxLarR3dW4mq976xkEJSZyAu5891UmUnsca8mw%2F1%2FHJRNedTkWYf6X8HqY24tSYWqfr9m3ET1ritrcmnpvZySjWprCdSw8cz%2BqnnpBbqZzoDagwzHvfRmv2Z1Mmy3Us393p0YLE2buGJmzWGEpNeNsHIU3d7zn4mxmrF3jk%2BuGxlTJNw0csh3rtTIGuyd1S4bAqZi8VE5KIIYnzY1KRwCGcqNKE%2BuEv1GrCi9%2BUNBJheMPw6XtqaA2OpDxEHW60jZfCXX6NDHYmbk1MP6Wdnbi1BjCAP%2FFa3FHAzrsQ5OLwNoLT53AY2NreN4kG0hx0tMNeT98gGOqUB1EWWQzKNaut0kUo9byRe9AuIa1ecx3UWmsqBCXW6cWaEN%2BQ5rho8NS1YmlPHYywgfX4vLAeM5a3MFadqz6pm2%2F6ITMvLg2wmkgjFQQc7HX8VId2HbZcN9mvoEv9xSOZl%2BgDXoZALfi2oe7P78U84lOKpsdqYZboQdnUgHbCQrOdEM%2B471Unsj7pZ7xW6ToBWd%2FtoHOpGQnwSXR14Xg15B4CWdjqm&X-Amz-Signature=867e41758e18419c5f561aae2f990edcda6be2918f53b0af9c2f294cddb6cac7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
