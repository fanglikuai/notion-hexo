---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IP7Y4JA%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCoDTS408ivHuZ07LKNPy5lYbTP6qCykVUU5QjDF82GLQIhAPW3VZHJauu0LYrMIdiPMSt41%2Be0CWvKp4CcfFhJw2TeKv8DCHoQABoMNjM3NDIzMTgzODA1Igwvpx%2FzOjf4V9pkL%2Fwq3APZzYYc3iE0d%2FBZTLNSxlaSyzyNH%2BhOWZQ3WuW8mMKH%2BXEGJLwyOBsQU6ziJcIy1JY5r1hLZWypjDYUJJ%2BPYTWrEoO3pqgt1qOaeooQqoekMh84LoGpEvAcmUNSI2%2B0GPLPoUL2sGnisgHXVF4ZEffYFUBbbcXGC9BmPWw5mh377vCbzzpUyYjglC99QDZgtWifezvmcW%2BoaVZAED2EYlk%2BJRsxxtgspO9nYG2a16HXUheZA4ICmwKVE3G6LXhimjji1xG%2FCHKGF2ErbUoNYoSwYlTUJ48SGswczqB%2BjkX5wNz0QDn5Q0qMt2gEG%2FaMsgyaauiucsNo3t1G%2B4iadRTsVVdNAsgVjkonYj2UwB9CNBcettlS2S2atmRQ1ysYa6X1ydpxjIIeXYeHLpmwf%2FMP1JT14%2BFzw3oEr8ZsdNdY1g5YUTGhBJhlqcXhS2qDCKbJAICZ55n%2F3%2FF8eL8GHxwYUZlUAIIMqOZXuGH7aPLqBvNLDqzZxdsb4HPzjFXhyOhF36nojiDBDUPmXSsYe2Rpk%2Bv%2BoQHntID8rQn7dSQm3AQaKjpKsPlgnFSZC6uGugMXw4iO20P36dh38ZQdrY9%2BBIJFyqOn9ztQbJawKZR9MktKsR9LlF43TggrZDDG36jIBjqkASMBXgUWuKsjnvwtf8ORh6yL0X50xoGBBko8O%2Fmy%2BSK7qDEvQtqepn8jelmRrTrmPJ1IBbunh0xaZGSi7u6mrxIp0kHf8%2F4%2BcR6Tck41DFy37mSbfcImWt5JzLwyi5EQp1FT9ucVv4SpywzC7PIDojm2%2Bm6IoVzMAwtoDK0KEANoBuRHeqAoVCWlBBLjkuS%2FARoiEt6QhOIezz2eq%2BOUIETsdo9T&X-Amz-Signature=996bf405eb349e7b2c937b74e8e47d06844a8f07db62f535aaf3cbe7c153c691&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
