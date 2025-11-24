---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDCOJYKV%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiNWV1Z89%2Bt0j2t%2FRkVq9cLZP3LHJG44pu%2F2R0YtRpJwIhAKNV6L7BezEtvu7jcuAFJMQn3q22XeZvGiISS0BV0r6jKv8DCE4QABoMNjM3NDIzMTgzODA1Igxh4CeqCdDfEWMMh64q3AOYbGNF2Cb3G%2F2qc6KdHunPJ0tUnzYLLMbn5nlokYoK%2FvjpFoVEeo2x%2F5XwOs2iD9JEnqZ9kltge7%2BGr0dxVea1RfUfMA1ORsCdWTaBcyfYy%2FfBPqTqlVTYu%2BJfkjCbN%2FuBuluCKVkh1aRkOc8xWvpRizNRSbgmYnhOZLf1zuz18KXT2rY9Yss6fKxBhmz%2FWiJlT3IKySEVS6Qemi3ZksKHJqtILAxYvgMemZnQ1VNjBLNoaq9QDyyACri%2BaIX67A8YEWhrdMY0L2ZI8RZ8KJQrngx0oK2ip2W6rCiTPRNzlPmZ6Tfx4EkDj29QavngI1TzqK0dFg672Tgn1d2TMWdu7EMTDFXxwJulMzTwXCyi5W9TbuJQfKV15rIhxD7Tj9OrmHnRED0yKhvBlSB7Ot5Fe2xPzjq8EFWn1hHlN6Kdi35iypHzwyeEQMO1Aja%2BtvlTXl6FF4z9zOLQhO47LX0Bg6UguGnlQaUd%2BeeoSpPjrxPiiAapkqYzAgE%2FGjAZH23KlihL3VPbNrriJR0qb3wZHfOLxfWulxBg5BuPcAEt2%2FjhfmrMQ%2FXRjDYde%2FPG%2FhVI4y8p%2BrXBxzzA3Bkbj3SvRMPUE9dXQLV5SnBLNU49qoZvGmICvdrPKAD2vDDyyY%2FJBjqkAanJEFQVmqbuQv3GVXFvhDuP5llxutXX0SiRnDcaCx0XEGRKDrROuMocD9hMNMB6BYp8G%2BD4fOk%2BU95kw1eEjfaOjuREXm0AHrQ0Rnt1kJwsUVBqYrdka25QbTzGd2dwe8%2FOJ7exQgl%2BmbttGcMUy64WUg6dtFmd3tKAqm4sStt2DPbnoTPHH%2BteSYfjGeCt7FZeapmaZ2aObQc41V3kqK4rvm1d&X-Amz-Signature=d61a8948a537d38c0f241260532514a6e3eaaa42695754918c201a857f7bb93c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
