---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDHL7R33%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIHuagCeGVPJmkqJ92hEnHVli3LVGAjiKlyCAxnsRK3OYAiBVSX7l%2B9KkmRiSfaDlWcr8g0DC5zEWxwYkpwWSlOHH5Sr%2FAwggEAAaDDYzNzQyMzE4MzgwNSIMBxELaOmc1HkMjA1VKtwDJRh6RT2fRRBBF0sHBUEE1RdZly66aHwm8KLU2gOMNsmYOzlG1zepbNCHq7qTE5h5ACfxUvCBs2%2FLJ%2BE7gluOfkI4lmnVtfFfEGi9pb73k8SUHrnQZGvJGEKhUIoH8vTbVx3awr4Xnd%2F81m9sQ1OXhfmjdUo2D38ximW5xboYCECO3tDCP2hmyl0UM9mx3wmSS8uqdxpnaQ6KvI077iRIvaN1WVxMNB9sh4W7IUF1O%2BeSt8jRUz7zBKbhTMrxanhwuCXgXQkbkcKmc06V9Y3HNhdOFSz8YVgRtf7QvfM5Vlp0SOxj5R8xjKo2ap3oz3Is2VMD%2B8CFYX7cj%2FLls1EXflKA0S%2FIgztuefKeYmSRQu5idwQgszmJm%2Br8Dpbw6G2wnx7w6lQA53B0RZnlLPadwevfs3%2B0vF%2BO2%2BmzDjb%2BFP12T%2Fy829zfyiQLznR0pZ%2FQ43PeSawhkkvd8qp4FG3sJIUkVbEe2mR3qVW%2FSlpW0IaueCuFUo2vxISies4%2FSO750js854X%2BujJCCuNVm3yyRYNcCIXRiHJHNF032KY49aieEVKkis3K5JIw5p69SEK%2BqfRWLt8A%2BHxD1Siv0sxIqF%2BQlvE8jMog7s9ov1uW6RcaXcgmknyMNM%2BUmmkw16jNyAY6pgF68rHCAjAWpUjnn8z6YVx4a%2FGVhOuwS9jQlh3ws%2Bcmy6MdXBrwD9tPYv6f%2F3%2BtIiCB1wmvEzNQQVgMJEz%2Bd7t9g%2BsH1yI0Cf7kddBrDrMcxC17WIgx8mM7rwqw2aE8o46pM%2Fv%2BxxVN6tdcJZQ9fHFfJkSIM9Eqb7VkJ9VazdMyV6Q9KJDbJOBqPWIUO027fDn4BE0hFf4zqibS5vSJS4ZGe5z6zZO5&X-Amz-Signature=f058811b1ed0eea12871ff51b706d25b90997b5c17d59080c93823e925a25c21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
