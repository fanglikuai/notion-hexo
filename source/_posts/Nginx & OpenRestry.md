---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQYP3RYT%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJIMEYCIQDDwR7TrvaHFll%2B9gOdZ3sUTgjVVDtllty5Zp4Qda4kbQIhAKGeGZJAWzAie3tD6Iij1lJhn8eY%2BwX%2BVVgXidc3SZr7KogECJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzuRHoQbblXoJB71L8q3AO0VWp1mlhV8mNLO91BnyYZnzlNTXkMcrS%2FzxnAYkWvv2oKwRmrP22wMhSp37I7Z6InQFZf9DBY5JGG%2BKKcQKjB0wsc0WHcqGE4JZOyJ%2FyUThZWlnPM%2FhNbYy629P%2FOIhp6VCb186j6ReC8DIvrMd1CHIGzzbdSCN3MU4Q1OQNXVkZjTxQcUoFaoRHVs35oGJ7TWyqbidPPV5sXmngJ89zq%2FzyWCXTWHC%2FgGqtSxMplTwmt%2BwxRmnBIKTV%2BAPScEZOiI9JrhH2nTc3uukqD8rk%2BJsVhGN1oXgh0zt5CZzHVUb6OMvOjVjox27Ij7LUCh4UCSZ3kydqRne8qG4%2Frjq7Zd8Zx4vO2o8kb3OvVKJs%2FNb6uBeJSnwfKb3HtMal0JMygMddsXiXZUjydkTuVoOry5fZQ2e54djdICkvZnCsA7Ff9Tt02cowZi%2BLOoDiG8UMjrlkdaTHvdGum1tKYH1pAvw7%2B2MzDAWtJiA9%2BFHiRrZgDnJa6L7o2ZhRsnHHzkgWKav9Yrt36nf6eUFfAqrg6lMi1OIpKv7wPdUMgIm4%2B%2FnbONfPB%2B%2FBI%2FCinRLKduMHZVVqydSC7QSoP0hMuY%2FwV2vnrII8bMk1ndpbdsOaQGcnLrYUnOWy3w8lTmTCJganGBjqkAVGTr1QtMt7Q3kplWhW9V6VAdpMx4xavceByQIQ9%2FFIIMTmQXTjRFlEHHguOHmp9mC2XQ2mKXzLlYKIxEHD%2FMHenj9VYZ9xyYoyZnMK3Pju5GPv19OLGdPNK1ZgniqmQYuFYCjt5LNXCm00ulob3C04x4Z9i7C3CtXO2MFooTPODOf01SWA%2FGqdzncSIQABY7MKAD6yg2Xwj%2B4FG9HlNSl1prIDM&X-Amz-Signature=a243f26a87cc377cf17e3088b89d3c8b75cbec569a058c7838fcf7d9c651e7b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
