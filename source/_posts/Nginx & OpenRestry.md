---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666F5XSYEB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIATSk00gEeuXl0GM6QQQdOzh4DKV9BAek2SsvKQzP9a%2BAiBu%2B0yjHrpWbBERLM5nTO5lMPzyJo%2Fyc4K2lPgvFCVtgyqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMX80nrFXwPVZco0eNKtwDtW4548qAnKqexKkoff1xW7Rjm84Vr0%2B02l6KMr6XRMsnbN7NFVLPdLdO7WWVM2uaW8ghlNXPaYp5p0%2BhkLzOcKNwRtyheRJLiqWs1VFo%2Bm94PXxehhFMkKP3JfjTZ48TyewGM0nQocCchZVVjXQIm6ZHFCx6FxpgYqAby9rLFSQNE9W8npNsB8CdSxAKiEebmIwkeEL02Nlb%2FSeMYRK1uatoqzAqAR13XG5z6EYSsf0zs2t%2FsFAo2KLpwCaE94ehZFvsB0AgoIk1LFh%2FRrrHheV%2B9FKLcyJTsgzqKH%2BGTtX5U05kwwATzIhxr5FPTkS79WpelSeJTW4VuproyKHF1uCEUid2cahw8PDvJw12H9wyZXLqng2w4eznYQ%2BHCFdqry0TGrUJCwpb8x2sEBuVZ4Ttcs3Ok9YJyw5pJm2iV5ZI2snkpPt1QbJUHSqOWduXsZdODmEgNFaDA0BF9RFBcjYSe3JH8IJysxba2fj9uxnn1hvPWhVzTWA%2FigLy%2FdjbLB6WSfhKb1EuLISVhDJhDfIyIcc6aV6Cgsj126KMD1oVnavuyBMZ%2FKXeyjtlVBFhGF2G6Dar5vV%2FZUz7Apw4SSSHW%2FgI6UG20tLXbPnjG5BaZDy4aOCNRnolGU8wrY68yAY6pgH3%2By5ff9S7ECtzke0%2B7DfCLfzTbAtNHrwTnvFTlYcik8x31NAPrazxHgawfs8Y95RzuArMDv3fF63qEX40oamQK1b4DyM50rzv3AQyXLgbBnRAUBFJkMvhzH709%2Bn1wF%2B%2BXarJ7YKSd2yVzG5B7fa9fDZo1JCpAlhjjdCDeMmr8alpe08XKYfrxOla0OjB9Lqo5dC%2FGkj%2F%2FmxgWzk9B0LIAU917n7L&X-Amz-Signature=dca22f2b9668e8a48390544e05b18f6e57bed527af20c4ab2acc3f211f5842cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
