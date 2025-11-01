---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZW3FTOX%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCICcdogTzK1PD%2FUqpSEbe0gEs6UEUfyqU6opetner9j4VAiAEJqnmwSHNgF10nTyPtH9ctI5BorXIJUNe9xnKWyFckir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMWdkuM1bU74JzxKdZKtwDcRHyLx9tfGI33jsf5m0V2hoP5B2thheQ0YrtAAEzOg2D3JmXH3g5gGPKzH0ufocT%2FjRNU8vX1OHNrpORrkNi%2FuvOxfjUQ8iMH3Tjc6EwPukciXKM%2F%2FEyXeXNmXqDl7hrv5464bPPEvC713dgUOtaTBqQDqg%2BFc%2F4Vd21d6QnwqMxmw2dZdNmWULa7oULuPsDuEW2bdgf2cuIOqt6vZMc42YoJQNEfCd1ICK3K%2F99MKlS1RGmPAWfXDXQ%2BBLaKp%2BFr2Nrq9gP%2B5eHKbTxg6vfjj4YyVRKzIzOhnGeMex6uaUln8%2F2faoCgmXtjfArbygtir8mbf%2F6ivwuIelLQzdzJw18alPZFTRMIPjLhDeRwFacIdsy%2BtJV5Jeby2oJPZ9xJuWPYdoQPU1WAIoTt5W6N8Wt2G48xTWpvXajplV629P5aNKj6U9ayCe8sgqQGaiKMTVVO5m%2BgG70gAdIyt59wPEbSM8At2%2Be8LxQsfBKnQgucXapL54bSNNNdDP9vVENrGM1pumx2VJZFfC8aMO27EktivDFSIk7Cmjkg3H7QoEC9Ii2hv5bzHWVkXSj701%2BJYTtNhj4BVHLD2KIfI4NrQqcQDH7xf2fr9gWnLNgdRpt%2BS9hSO9ieru%2Faocw88KZyAY6pgHBOqiGF%2ByojmEykAy3ceGIYvhh0jMenSCE7lz3kLsXcuzfpRtbJtN9%2FMzIW5mrL%2FE69Bk%2Fih%2FZIdZdSLdtVpZ%2Bg%2FTzGg3TzFCnUbFnFw1M0WmwCP5E%2Bu8v05HX0mRy4e6bdSRcXCbxFFzjz9YLlfHB%2FfpAa1lAl3XGDyqYduGoKNFev%2BGNR6vw6hERObS0GFxZXSgd56CE1bLPsSI7xGLAp2rsUvH9&X-Amz-Signature=8376d0699c2698d33f06eb52fe8b5dd2000029c363f6bb924097ec5fa3466aa2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
