---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPFY6JKO%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIE6hYri548FmR%2B%2FHCoGBTfAs5cL6cC088pAP%2Bwhi7gTdAiEA%2FbVDU6rDcGRoD6%2Fvsf9p9DL9IDgHQfiTEiqUpZFM9gcqiAQIx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGKysoQBqDBIOHZKhyrcA6hSFsY9UHIod3hq4bCFJOnDZ9LhekF62Cgi958XvkGFXJkQMgbKLCgAOHi0SyKpoLi92CNqYPfzQRyfoBO1TDqhAIu%2Bse8k%2Bg68uy6K7%2BrQjEtdIM8JPobUiLKX90ZRq9r5wvpZFs6LxtR0q6rUnBcB6mYQqig9k8uwp%2BT9ABHYGmTKiFibjAjS8XO15DZjgZDbJ0rCP0Rvkf%2BrkWI2fVDrtVhU7uS8Yy8iwUqWwdKlBpKjBtGXEddH8x8r%2BhZJK7PuvkWJg1820cBnOSIeXXkFK%2BkX2cYR0OhRlP%2FSst62Z86oskPBCOxHvOqLYLbVL%2FB2jbCKnvSTQtf8h%2B9g3PcxHxTxDazwDs6MwpKMuz8iHXxAz1bp1VIJigWt%2B1nSrcu%2F7ubjDSlIwDkPU5ns9%2BRao0WhZQaK6U76WI%2FaXHKAKoPH0JzVpD98Ng4%2FDMvfE%2BrTrrno31vRoKcVEGMKALnyyOJAyPpiA%2Fs49oexaQYLMP8HCjL58hAMnj%2BRr7WyIaejRnRvLxrIf3HMubbUttd4wDsDJXuRO0%2BYszbEXBfZjqzRnw9l73ZvZSN3gVg4mjdSA9XroX37ZKRt6DETqUgYpx9wLXHgK3u4dS5Dbg74Fih%2FmHLaup%2FcxdmqMLbDm8cGOqUBoNbID7edYBPboKd%2FCLupdxUDZepY2q4SZ9R2Acm4SkqD5KhPoolnkE%2BFA7AGdspLWJPeDgkA5dloX9JYIfZy%2B7mL1VlqwygEkecS0M0B5UC0T138l55HMXqFPhW6DgG1WkW1DXT4P0mjuA%2FbEWFcAj1hsDNH4Ov9fE%2B%2Bpd9gZZ4H82u%2Be6WlhpcsMCfzx%2FhUMw6VAOH6TXRRDkxdv7G%2BUhmMQOWq&X-Amz-Signature=90b23560de8a084f91b80692381205119ccde2b953af76fabda145e26b3673db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
