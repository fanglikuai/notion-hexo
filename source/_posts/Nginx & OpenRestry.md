---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466672LNUUU%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T210036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQCb%2BlffLmIX22TsAHsadPEZzDc3EiIu0uB%2BQ0E0TIy4DgIhANrZlKcsnGuD38vEh3wiCL1eChjiSFWDAU7wsY0zPCNqKv8DCDUQABoMNjM3NDIzMTgzODA1IgxwAzmaOoGZiZBZWbcq3APOThzxtIBrrS4GX90Tqfmh9HmLgvKUVtPdF3MaE%2FZ4OVyIO8pyzXaim1IX1nX5yW3ZcQhZNt2tT0sY4wFLNzfaGNSZqbKcN4v4Bd%2B1tySH8NAy1pfyhFaXVqVyX1f0eUKL%2FxbP7%2B6ouEbRmAkbIb17Trqw7MowVYccfwoGhQJYuOQxYV4mQmIMLD6V%2FNJMLWPp2GpgmmTaLm8ViVXSSRfoLyMJOjVhkXHDVhNDL9Ynr71K4gvbUknrlJDkPTHjlSGmuQ1mMImXJjQNe7c3BOHkAoVN65pGx4FksU2gpH5RAQV3onuISHct%2BTzSHlsLijPO8oMUTGm74LzYo2NiIvPX820X1EkoOfZwUCzqiIvZZpfE6nm1ixLktQ6FJe5WelrHjB3mVaw2xFa%2F1XDTPK6po6cpRCIPlj7027IBYfL49Al46Wp8D2p1qdMdl0Izwa1G9xZfDC9nqxKQ1JrNywvsdUnm%2Fpmg9y6OcFCJ54JHd%2B81j738gHEtQ0MMocf5wIiXmnUdjOCEXEtDzT3DonZ2vsrEbtmqIfCd1TybJLDElukYBDoafpVRPt%2BxtRlxskPz1fJVy1acxrpmD0Me8vwl1t6SqahQNsVopC7puXttvtBNE1%2BgeOsnndljKzCiw5nIBjqkAR2leRqARulTwk42pwEQ76BaLkwSwc484OxYEharFRhjF3vOtDfcBKQcVkzhd4O1ADDv7Ihl1BVIihoUKKq17cbt%2BUak%2FjTOXPKyhWj1UCfXVvVq9ZxBOaC0%2FnZtmC16t4MVm5%2Fyq31F5CEuYuX3qbpqVNP5x5q2DVgeA9Qrhts5Ueu0t2KHc%2FYIBu9eYR2sdne3rGgSp4%2F3TccifWcQHe%2FBKSjl&X-Amz-Signature=9970ca9f63a7888d391981a6085ea304b699064fd9952f25d1d9980bbccd7179&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
