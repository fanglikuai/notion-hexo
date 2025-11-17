---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFM3UT7W%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1%2FvVs0C7yYyipem1DLej9dbuBvzT9cCHmDdvbpmMoywIhAKFQWtNEdMrH%2BpboFM99MO%2FxmaYXP0VBHtmZjnUiFJ5TKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwmMCxTK6d%2B8Cwbqgsq3AN1MV5xJxdx2JEi6VEjzSQ7Ng1usA0tHHUPQ%2B6RkSHWtgxwrmrXwpvMEbFLfgLbLV83BghyjJ2xgGstXH1PCpNN53cw6Nom9AqIqZHfZAoolorMbsUON4PtgTnf7duz5ZsedY%2FCkU10ZNbD4Q5%2Bp0kxJMUDRUL%2BLbTr9ViMCTo4PI1nYIcCdam7o7yhSI6SapgrJxZjiBHPBGPydsj9QLoq9QrJQMdzPx0e89nu%2B67m7DM4PyQXiv3FnkwpnhVTEgKDhkARD5S5K6mKmuTIkLKivt3gx8Z%2FTO29dXmagsVdXUoP2XhjDtNkysZHLXYv5E7p1sd9kavDFPNj2z%2BqBFyHpCkNq7IYAqqH43osYixOI0OlsamM1bzc%2BABpERAWdHNgcdKrGJ934KjjM6h3QHZ8mRarzBHU4HelVUAev8K8dnRWtABCsKIW39D9IkxbjZp8eSRyWc7TNM1a%2FNNR7iZHywgPxpyE5Mz2ZY%2F1GlIFE9gsZvVVj%2FjUZV7%2F3u5ybfnusoe7OQsdQqZswEH6u42O2pR0iv1twrO44Z1ELkVkVzZVN7RuZ5Y64gPMCKY%2Fu0KuxLs%2BPUuCRpvhN%2F%2BnPCVLuZZnKBs5sYHgYlsG8LsqykJQTVn%2BPVyLrk%2BuszCQ7unIBjqkAepid3dT60krYzjDeBqJvQO77Wi%2BVrbRcabUs6BPlZLODV5RqaPVvqVSDfoaM%2FWm29U%2FRzkAhXJAzdH7ctzpXL3Uu0NOySjhi%2F0L34triX5w0k0mMqn86TaIrcEUwkZoG2OqWAn9s9aFIBdtFVQ1mlpV9WZOqpIwEwZXc9k7fBINl7Vl0SmOWTSE7i%2Fb2FG7aL5iWthsKpVYw4PK5mqn6XBHmlRo&X-Amz-Signature=b737db3c4db69276bde63a3f3e35ea8b053f16509ce648d831b07c9dcc016063&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
