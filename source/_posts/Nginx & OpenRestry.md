---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKYAQJGR%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC4igPWfU9t78xsuvfooNK%2BczfrSC%2F95Q%2FHtupgFx55tAiA%2FSgHQ4BvTdODy%2BeagYEdYM7QoSqrCGJK1jzMGl6CSVCr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIMuBDWPzYjP%2BmkeHOhKtwD5awBxO9Xmkl%2FijmquThX8WHzNy3JLSS4S%2BxHcqN6GZ6qrFBNI%2FtrKsvCRq%2F84XTm5OGecATkYUL6G9fwptWXYJD%2FG5%2Fy2wm8CxtCxeYU%2FRb1jhCLgB0pVBQ2YaSUiQiXqOQpbjlHEjf7DYAfMtUVQ0C%2ByZ1r2KVVNyW9TseGAAg0XxulQY0f6uQm%2BxC7J5LQma7ahm531sQigiLh%2BYrPjeAxqrihXmcTeD0PzM2WhDc3xhOL5TuAdSJcRffNhrHzKrkXUavNx6btLewNo6VpIj%2FwfIrTd4sz27VM7eugRjlUMKdlNZ1buJNLgN7harSrnJPstNWBxxoXTvwv%2Bm2R6ulttwFAAV2Xk3n91LO6LULbMSSEB2c2pPrbb3BNOqKqHqD7ZGIvcBubvKYJt8IexcYtl2qoI3bKNCJ0mgASxNh0YLjkFALbcyFraHqh%2F2m%2FMyF74eX15EHy4TO2dhHsjC2oXHiO7R6sqH2Brjd4c5lc4RH51qVLj4bi89kaHV%2FbcernLHs1Fzo60qaLzt%2FVD8HFnv1acPFVkBTtFjnvCpod%2BLGAcAQI7eVPrVF5ngaJ%2BesaDv0lmtCdnAUVJ7Mj51bya589lUGh1PHnq5jcRB8nmz8SojTdq56Hvv0wre%2FDxgY6pgH8J4isM%2FTyY5oZv4jahBnYCRrgr0StCm%2BNCATjOUG9woDDnIeNtcAs1NpG%2Fi0U8iikXXwH3PvpHMqQLW67hfKNgIMYxTkR8FD5HriWCsB3Fl8OKzb1gbqPrXGeA0bnzeMdS6GMFdWLt8rnavIJ5eH90aoGIzVAoUBrBfcxiEiN6CK9J671hkExOzE6fdUmn5wI96o%2BXkHm9KvJ3GQsi7ME2uY8%2F3%2B6&X-Amz-Signature=bda1ee0ca45117922c9a90e7bd943cb5644a2bfb26a42eed10248b6af7b42ae0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
