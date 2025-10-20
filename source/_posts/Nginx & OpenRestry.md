---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YTHIHCFJ%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T080507Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIBoMu5KVi%2BReXz2ogZePUrcbyI7OjfI1HH9Xnbe5TTh3AiEAlSp9AiJTp8NeZyRkEvjvCXviji8Zui7B5FWHrW9XsMQqhgQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEb7woku28VFbTdDNCraAwp8kUcYTJO0xWCCA1wq1G7I%2FC1Z7QV7YO9uLFE7kf8f3KRqPOcYvjl5PhenNQWiFpTidk0njSbj%2B%2F7QcwpC34O2U4vthsej4ZrMxSHAbTASIC%2F3aFGD50VZb4ehGkzI4KeHzp3E1rpw7iLVZ7CBYuGTvwoqBkWaSdQ3eIRaNgE63ubYxH3Vi5xckgn5HPAA%2F4RbOX82RCm3yzQ4cJ0tWG4mgetGote7Y%2FybQdJpqk8mcafDwhjdqTxWDYWFyoKp0JhiDlIHw3JPBwx4A3%2Fs%2BdNSNiGv1TkHtMEzA6VAKhVG4mQjuTRd60CyCW4JT4tylIXoKxHoocvuR7TWVAC1CE8HHMVwoS5rZu%2BePLLJdRY8CM0eQrqT9YgjsHh5rqokUbOgVv5lDIOuTofsOwyxt8Y%2B1U6gMdcxhLnA25k6dU4ThlDlgxfAwph4FDprXWfqn5SiG2Gx54SXNgFpzae5WhWy%2FA4IrB0uFzW%2FvjFbRYyWUNNk9j4Bs1gX00%2BMNDFj3j3UrKLoSuqWr4wNvY3xfnPC7YYzLaesZurtlH2CF%2FOwhqXlmBHiz1YixLK0dCY9KzBORlSaEa1fQJC1OBQUhmoc1Y16Jn48%2FkX4rsh6dcvcT2vHM9iX1fYy9DC%2Bz9fHBjqlAXH%2FrJhcDq%2BCt7ueJmb%2BRCwlSkIIevriDc69IBwj2Z6wxRh66A%2Bus%2BNG0W23S8qIMpY7EHDQ2nTRLPFGOAEJyaxN5CGuh1x0fBzy55%2F1g2%2BR%2Fl4UsnaNhTVm0kGFh385bN5N15sInriLDn3f9FLEofsyOESNU1gFNv5cwB2m9GnLBA1iYNlOPHrdeEd90cy2TDAesnnEYLkCM1FzA8IWgrL0r8Y7NQ%3D%3D&X-Amz-Signature=9df25fce54466c37fdd11a6b5db50a56bf7a5686155d5249f3bca65a9d9b0db0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
