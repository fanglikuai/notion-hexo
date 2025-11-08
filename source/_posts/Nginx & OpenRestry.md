---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TQLHMA4O%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T120120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIE79TOYD9tXCEykQVLD2qhc4QujYoq%2Bt%2B2wxEGZhubiEAiAlo5FCtm5slE0yzFeqX39JWDIPJY4CB2LhASmQeRV3qiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLIyAlJvnLn9fuZvVKtwDlCtB%2BqzbnjlaSnnsG49a47Sd4cpUqnlFj6gxUjRCyXJjNzfxKGDTlrxjkO7MWPtTxzYlUm9HQewu7WHPgsiovwZGMyk1rEymSOH2hKKTuxvtzESSJsvgAZWbu1AVB3nZvxwuC6YAWH5NNNh7yrkK2UjMBfp69NqS7agx9KjQgQyALWgRXL6pRJk8XNYwwz%2F7JCQavm%2BP3nU9yo8jumKsCA2kM10bm0bnQAM2LTHGo8kF9nk%2BgGWnAGmgaquFRgae9dHuQFYCjxtlQuQXRgdYAUttGwfX6Lnc18JxXuxcI39GaegdcCDIeLRg6eFJWA%2FB0etenlmlxm4aWPjOi%2FuNz6iNJjFotVT%2BqyfejleIeLggJHQL0T4skhgotRMo4a7a3iSR04MBPvRYUhw7eJglaKL8GKyQKOKQ%2BQ15iYnkAtS00AjO2q9EQWdNxtvNQi%2BVwt%2B1FA%2BJu2yDH6JfcljoNjGnNU6A3%2BIgdz%2FXRustiU1Oc9H025e1TjV%2B6RvyuapoHmx990T%2FGRTStRqGqC8qE2hnOMfWTzxWDhjF05TEcEyB%2FsZ9LIfOeLBs1YSty8bfOh4YiO%2BdFHX%2FEOYS4RAEXWNIJ8zemnbjA%2F81d3UxtOYPl%2BTlzFXwo39l1e0w5428yAY6pgFjse6Mvq6Bxg9wuGmhvQ8jjO6DIjs0f7vp4O1yy9Z%2F06iCPduY80%2F7C8oEOyRiJtz3AWIx5PwwhoNSX2H3r03%2F7c5bhvrXO7r0cCBeTZmXiFk5HHa%2BPwXCyN9j%2BMAi37OQBCh9MVCki1ChQycEEyyp%2Bpyq08qV4rXNkKvwHhcjPn%2BzEUIMli0%2BdjEzVarwmFs2sehoPwo2Syqy6Y4ud2f81s4nwtGs&X-Amz-Signature=0bcea08863eb2fe700aeec7b6e55ee8e1f86ff9f0d6d438329335de39e2a3752&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
