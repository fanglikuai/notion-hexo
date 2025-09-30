---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WW4P36NG%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQDpyjcx2fOwgBxFt%2B5NBsN5g5ds8OFMIZjm9pSh0fDoxwIhAO5qzFoXreHUdTcBF03%2F2%2F1zWffK2ZGnbUX0O1qGg7hkKogECO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx8Q7d8E9RPdtlgo%2FUq3AMwUDk2TWq8FKVIi9HInALS6Ib5Mid2R%2FoSiodi8n0PazTxwsfX6ApFSe8LGZ0FXcttYOJ0VwB7jOyAif1UkUTefjNVreSXKuHCzKVR5wSfp%2FgHOB6FjrEl4zpyz7ueoSUH60uy3OxOEw6VC3hJVgonuzAIJc0tZE%2BnAfqWTj6Vyfyqi1y0yeDTKKG1%2BlmQFFuyWvCUlnfgKdVn0qCXXtu1lOZoCvzuIHJmZzCnYXuAXBqrSgDKFJxEHyX1hBAkhatdP0qiKmIKyd1OlAHUjyY%2FB4DDPnu3Rsbi57pT%2BLkeyKws9JuxWOxUAi%2F61%2FnQj66pT54ZT3ybNbcYBNOHzS%2BtkZDcjRzB%2FvXkHo%2FjlOQdjiWQr6Rw8wUInK%2FijL8c1JNuPA8b2j%2F1M6H2Yzun90WqxDmQablCrnqEiwfqhA%2BogWrKgQFer%2FctLaXWoCG3B5MvUYOT37kpxOyE0iHkZtEmv7exfni1hKxy3XM6qy2dGq89%2FzjmK5wEb1%2BA5GCdafnDB2KTB0XlsfqZIZQ6aRb0v3l8PdF4LLdvL1j9pfzo%2FG61fH7wTVM4LWrnFE9usFpQCRdllsAcvlV6dizNc9xcXJFax9nXp9Y0VrKKscQQFQY9BnOtYa9a7Z1sxjDsiu%2FGBjqkAVt3xXflobQl910OzdV6PY2Mh2ooG4CfH%2BABIODNueXHuPZze9z3J0AtlSqtu%2BeyBEkZkKLymB1feS%2Bq2vVIkMrMQUqiJ3n15cxIqf76B7TijfsHY8Sn4xkMO2JFzX5Qdo6nE4IVrDBpagEeZnlbtitNqW3%2BUT8zxp7wlxZJU2eI38%2FxNsPdozEWmXd69wuaiQodi6amITdp%2FJsMrX3xHSWSwPbh&X-Amz-Signature=6e3d1df9fd8e5985fadfcc725e2d9981173410017c4b8808edcdcff4dda9289e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
