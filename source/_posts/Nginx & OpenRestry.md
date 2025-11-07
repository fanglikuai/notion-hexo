---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6AZK3QN%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDge5mmOKqses75ygEDuZJ0jmvHhz%2BLufRn0aOv8cG%2BvwIhAKgg%2FpJAh3Ue%2Bk34FQbzkxniDn2h3XGqnuGTPvADK5F9KogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx4idozHZSdzqFkus8q3AOXxtoGiqdsovaluFdDlxJ3%2BpcCkXlEVVK%2BgGvPftlXzmHlLwTaCzc5ENaPXGn4QOr7qgrX8rrv%2F%2BXF3Gq9zClVKsk7L8HV9Ft70osV5aOSET%2Fwuwdk59aFsTCwt3AlYaB3MbH3MJn3%2FB4T3HidhjuDXceQCUJuM%2FqqaLsp0t3WmuUCsxtJC7UE3YKDO8dksqvi1iqV01C4bdLy%2B4Ty9HH6khoUh%2FIPMMncwGKhWQGOjSJR7%2FZxK1Khg%2BI%2FRzjCHwhnXy8mGXILJYy1aLp7N3%2F%2BmO4ANApEIt4YRgVv7pMPpQd%2BiuRmmpsh%2FV1Dz2C7uR1K8qslUd0%2BfbLGlNzeR2GLDp%2BODKk1mugjaZvFJCP8m2OfVE4MsINEJKaOmDCP4A%2BdYJPtDGpIeDvx5QzpBRTvOs2a5cvaDushdW0bgdYMWY6A7yJgHu3%2B5DOQ9WAt95PHSiG18%2B0ZX1%2FpgmrqR0dePBO25ojKFudNe7xMwmEhtuAiz9I8w3ODhuN4g3R1P2fXgaPHuHouDsB4d094QEvlOzfVPu8fGmT1lgM1nKmTe3jWHcQt8D7hge50%2BTesf5dEwUJKI3RiSXPdaXMzTjWILScVobjMUmWRdx6xMRk%2BYCDBu%2FioqUSq%2F0xzFzDUsLfIBjqkAZmRDGo4CiPHqzdkDc3ljDoCdx4bjRKoLgE7mylX2YvVCE2DNBtl65noz7yhHMNOeC8uPxvH3eyvw9M2JxUvVq1RJ3JOFG%2B%2BKZVOAh%2BiYxsRoZVKDYCzJshWu0YmZid4RYrb6gyHDgqIa8iSWT0UP5sHv%2FIrB%2Byz%2F4oCE5FM28IQIqvK7Qw1%2BQAcWk8AuOxEC5r7%2B4ftEXYO%2B4uNLS4DSkGnMWwW&X-Amz-Signature=1fa39aac42b046d3fb148a1aba64ef29724551647b1be3c8593a852557798e21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
