---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IGJHWF2%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJGMEQCIGqrbsTqAD0FHJ8C41jaW2E2zdEanJQTukmqA6lwNiK6AiBbwRObZYn3ZGEbuRxIqrBpG0RwkZVo1MkbBJBrkewXgyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMSomKtL85yDrftt9EKtwDZOX1ulUkRh%2B5kde%2BBbQdAHqeB10jb6hvG5ilheeMz8nbMzzKFMVKOxkP%2BeqENmW94EMd3IRb7LUBKVKobHwiouj85CQul12%2By69mj1FohtZ9dj90nz31bFZfvhqK9NNYQR86BmYulFsvZfubxvOmuwVvNpKLnueujfVqnaPZJ9tC48hiuwxXwCy%2FmWaHDNNMXsAHeOZdbk%2B6JDQZ9JlRdBPRF%2BjEWKJA3K3FcRv3CLKWYM9UN0NVSjE4apew25rKni8n9NOoQk4Ln%2B4n0yvueGBRBQsxkO1Pypj3GbUMzCHlGdqx9BFjywh0Y7zZWseWryZjD2GNx3Xn3SjWG6mYGdFaTTrS6Zv6xDCgkZDmrf9oVaQ1mB28l10b4m4cUn1mUmbCKwwP6vzPZL9zSgBV7eKAWhPTyQpjrijwjtL78AgZiWNAlKt9E4cc8IuKaqPrX8dVBVb0B6pbFtHDqAi4xWjc12g003LP2be4CeBDMS33DI3kS%2BuAzLfUBfem1IH4ptPfzTg7lYLfPzZnBK%2BvNth0v0AohANMFlnkwjB1PLZXP%2FhEZ0iajHYqdD9aF2pH05sRYDRipJaGArl8c6BFV%2BxNr4TcqrihdyOZThQWKV%2Ft8ZOB1jzEZNDaMJAws5zMyAY6pgHSbWEC6TwAjtU1sUQpiK6vCE4J3RE6WrtLW4KgWjwHQcrmzSkrTqd2JJZdGUu19GfLX8kSaTYFMELjlWTJB5OrODQhIiBCY5%2BOdV1hGitYImaCnDIh0ETqVrNI3MV%2FbRyujdFqQ%2BqZB8oJRI5FVN%2FejxISsPyFTxGd2OGDs6WgvFW6AS1bsM7506p65BpUaFKmwaKLGcTGpciFRktT4pmKXQA9IAmO&X-Amz-Signature=7b01773bcefbe89caf7e18b69336bdc64dbd75d2c1064f1840d34fb10133dbfd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
