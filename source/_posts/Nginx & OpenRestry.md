---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6F7ROUX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC1liHvDd8HCs9KqQvqSmFc0Ena2y2NENwQgKtN1bgapQIgeYYtNhWXfAMn%2B3NGoPJElwqWdBF2pG2QnKnIAhk44W4q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDC7dy7348baSzsMkNSrcA7C9u7CkQ02h4zEzAbOG4AS4fVXJMyvSrtZUck4%2BMmjUsKVWPPStPfmV2SOl%2B4CMI85NyROlrAcB1dCuXI6Bo7TU24BIlkVNrzR0FUjNRzYwzCt61xOTgJlBtGPhrpszh7AjRFExF%2BJ32d7q%2Bb6OsOtkiLztqA4S4biLNlndJwQGdMNe%2FqxNoynyW%2FugWx36m%2F7eIV%2BfzexLKGeC537jqX6wv4CvDdktgpsP68xnOFFInm7C5Hxn4o%2B%2B92gBhQbjq1uF1EKbBHg1A3yzqsSaLk1cfgNQbyeb2QvLEsSQ%2Ba%2F%2FlMKRplg5k%2FamGuATJol%2BR3v4pMnkhmhQS%2FDQ0s0ZHJowl%2BySlPsBY5teRApD9F3x%2BKWyVFKoPFMI409VdLgEAE8DQFDJlrVd7ly7Ev7xpoen%2BUSC8%2FEwL%2FBDNzcDBBiWxb7yFgTTpHXUVqELOSwO2D8bwEar%2FAxzjlwDC8F7Balf%2BRjNEZueEmT8ucjE0cvoOP7%2BGkUEp3uD1yENDVbsIsar4reOcbFerga5fSKLS4SBo5lGq%2B9SyCIgQ6tfrFu9hM6u6T58UdW7HNgjp0BUksnF7NDAg8LUMM9hFjtUAghK52tTM2T2pS3tNjHAA4aXsLI1rE%2Fu7W%2FZ6NJHMP%2FXkskGOqUBV%2Ft7olwhvjU%2Fg%2BG8tL0ceKzHsbiBCIQqDzSZgAFKwCCNDtZQnhOtkLsxqyeYQ%2FaGqDk0UkXvdf5bFGzQ88ioA5s6r82OGTg378TLxj6A%2FLOuM1XOYnASpuQYdd9O1UMnGLA0GbgQinSPfAivR5F4dfSDzj8G2n57Uv5ItJXDx2mdgx%2Fi4pkc1EUCFVrvHyXpSYZuMDPC0lgEXLAtf7pXI0el5FW2&X-Amz-Signature=bd4168261722035bd9e6d116551e52be4c2da2b571b8abe3cd355d9f3a680558&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
