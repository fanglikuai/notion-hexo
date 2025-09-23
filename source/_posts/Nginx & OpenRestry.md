---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSSW26K3%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGC7wA5t%2FRSZNaz4ogID41E5%2BkSEAY2AiEv5Selm6ALAIgN6AMYEXB0i22L8EOViZv3cGTi%2F2EX05s0tHb%2BsV7YO8q%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDAcTSGl6ouc5ysdo7yrcA1yELaW5%2BsG4TM4yYTs9hMS0pCvpfVdgJVlOQU%2BfHstrCgZxiwGC3nINGdXhRYCTNxcsYvWnjMlH2Res6ZlfaGuw4g%2Bwye%2FDVOIdzncZ5feIN3p7cmcbvX1vor2fB5mV%2BmTDBah4tQ2uBBexns%2BXfe0DEC5UdQIOIfoZHQozv9rsKi6cKOg5efgdb2I0k4KQsgzyVdP%2BP5C%2BTY9IGTzi7NQFrUIhtVLE1V874HmtUnc7VjcXiyVQdr6TG0d249mdmVQpSW9ynBtMhFv2TCebW8XaQMs98Mf3ZU2JUpkel84CWDvM4D3xvq0rZgP%2F1nSsiv1SmCytmzC%2Fpaq4N%2BXKt4S8mdI8ljc%2FTGi4oNX7Hyrx3SkMdj3S1i2EHoJ3YGTGoWjDI%2FvLu815G6gSTWlRwzUWFzxQ92WNOXaFkTbo9mTg%2Fxpq2PIJjkLT4csgM94L34QeFSegDdXfD%2BSpNOD6Dk1uk5JBvyEQ4YUF%2F9zkuUEsLP2uycZwUj4%2B5qRbHqBUkAKUcNiKLF%2FT0gX292PUxq0E9%2FLZ7DPTQD%2BTy8U3AaZdT%2FexFXyv59aBmM%2BlukN3Ct4%2BRtDMGF%2F9vL393W9bhbPwKWU7geD%2BW8YE2Bu90hGr5XG5kVm7LYGuz2RFMN6ux8YGOqUBkosPBF%2FeFO9x92PILw5hsAWqTTqJwtfeRVPhboPoO5ff%2F4P5eWQytHAmO9WmnX1DkoApHJMGFis56WU1TH5ZrrZtN7t%2FgIxGZKPWvRd1a8RRk8462vpqYKAK7xjqA0sx6sVePVpR7XcKte%2FKVxjdPhHAnJQ3a%2BxIgJT%2BiirbQS1P8SuO1smxg8nO0uSTkxnIBg0ZzuX8qveMSpcrtCvHYSBQlqSW&X-Amz-Signature=837df665c418aad5f6503fb46e699c9567a88d222bbd93c32599ed5a288a2d8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
