---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLFYAIAI%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJIMEYCIQD9MEye1%2BjInCnqzWpInA1z14rNneoQMRN5LrH3rWfyFgIhAIN2qFFQLLZ%2F3QYEkWR84O3u9D9MVKyzIfCkLj%2B7G8jJKv8DCBkQABoMNjM3NDIzMTgzODA1IgyG24G%2B%2FLy%2BKht4h70q3ANu48FpbJKTizUhNp9MeE1RE71ZcXwRIJWp0l%2FcNpIZousbbT%2FN9gTkIeBLN8pQKPCWzf0ZEsSd7vtOQkKz3ErGzfgAlmUBK6cMOfFSodiotW9a4poLHk5Jk5XmjD30iD93VB6S3OOJxI076ALjhFmNSUmBLXIoxWRK6PxtkaRCnPcYFz0VhYZUFZ4pYCNf%2BB%2Fsb4eaiXWjMhMT8b4MkBvQDDh9lofGZGlviEHDzZLKvfTeptvKmqpMxHJ%2BelseFLD%2BFMg0%2FqvQRUMuXEUvqsnIPw4994rMvLU6v6DKW7YSZbtATrGmJpNaJCf0DbrGYjmK%2BvlJnNN%2FVeUODGCUHn%2BnGRxXCvioIOi%2FOJYI22IPj6JNW8k4D76PuFAlc6iWzmvMgxjMC%2BZyT97Q3hR0kyCvBgr%2FJKRPwjJUKO6yHK%2B45mSUTYR0fsabgy43NZs8EEv4X2hY5il5%2Bd3baVpTGTnBt2fhRojwkSe0b9kMZcXK5ao2n5MlwSJWfAaV92ing3%2Bg5xIs0liUnppy1NyJATxoMgnUPyvYJr2QVL1UKd3%2FcbLs2537PxFVRQH0HG4gPx9cDekONLAxU3GrciqH7yVuAjm%2FvYqf7kW44nfsbPkY0liwOAXvpwTSabK%2F7zD%2BuZPIBjqkAZky6Z0q18w9%2FlvAAJyBnVs%2FrwkZt08td8d55oSAzgATR48HYPK7Hzk%2Fl1Cs5Y9VdUCyKvhCbLQBYD8N6JQYvAwhT01LvhLSZYBB1knH2uoEwiVN6ZWk%2BU9L0ph0hutvm7H9tQZVLx4eUizWMM2zEfhPraW8lqjZVxJEcJ5bNdhLfymgvm7X0NZznzzeihgh8o0Ze893vg0yFX%2BWPCxcNBefmE%2BZ&X-Amz-Signature=d3d4bc09986ebdd8cb5dac5c180821fa72320fe7e3e30971df05df1cc1c390cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
