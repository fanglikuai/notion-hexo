---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THIVCJ4D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtclKrU8SvaMU9M00jjzFOXRpFudG1F8OgrAs04iy83QIhAMOFLCYPh2Z%2FmAn%2BY6lssMPQcTNgs7SpyrXPRIUpfK89Kv8DCHUQABoMNjM3NDIzMTgzODA1IgzrUzLzUAK6at6yK5Qq3AOPVCSiXmhEBCSzA4v8m%2BFrSYXJpgtgSc%2BDKkXP6BMD1bkw6h4XSQhdys%2Bzx1Z8JEDbVcMoWLtaWPtEgg3NDU4M1ajxtROocCLnf6%2Fl8fA7CayDheXE1cqy3FWI%2B6gHOESZMWrniV2JRRpRjVCLOf%2B%2B1X5OftHrYP%2FECHd9O7ZYbNJMq1FFt2EWzGWIHM2nXtc9eBXdAH9gqY3ySGZJZrm5nKZk4OtjLGilU18dAJlTyZLUC2FBdhv6Q7IQ1bYL7c4LuW5pdSoEFyDbPIcEAGc3dZiEM5%2BQR7IuNVeSrPtkkHihbMdTtN%2B3hUUAH74q%2FQyj%2FrEzAB2mn4krPu5nxRS4JylSb64Xb783q%2BLTBE0DIFKnGBeadNeZ8zuUAeyEqLswXmCEDEksagVSIRLqv5T032jUSgYqbrgK6I53mxgsueEGtcNf8hibcJc1weKnMCUqZASooekjgCjWjKRVbxKE1CZSeJ4xHEs%2B%2BDTgZ7DiRqVLupaJHLJhFHVP2YMe1uz0R3ZemOrNR6%2BqoG%2FYHHufqvBoWkK8%2FKNqHa1DrAm5UHg9Ve0qp0vlDFBLvEqWobDPAeUd3T0X6elSQBiOLpEPREFJRU%2Fp9gAz4bE6JIEM06qiA9nNxUJURwQCGDCT0afIBjqkAaUGP%2FkdKT9TwDvo8ZG2hCiZRgfM1tVlep1u4093WPOFi6RlOvqBdYtXA4k1Gbmio4Z53JymAV0JJPJRM3VsSxIdH2FuMS58yarMpqK%2FIRosDNwd%2BiCC%2BgbOnDKocqCYG%2FXSnqek%2BqXd3aOrEv1uZZvYwLhHK5MsWj8G%2Blx7Ad%2BLxKrJFoI0ggEnV7SmiEJFJ7AajexMX3PBIB0xtZj0o8vS0NGb&X-Amz-Signature=da6790829580fbfad20b8b85c049ebc1c49bbc21c5a92ccfa06d1088c6e31f8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
