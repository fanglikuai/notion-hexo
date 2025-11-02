---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Z44WPFL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T140111Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJGMEQCIA9lXryYO1%2BIJr7HQW5JpwKNwJudN7A5sAc%2B%2Bd04nSWxAiA%2F0qaCINAFN36y7v%2FyK2ma3TFTDFEG8TaXfE5ms9uPfSr%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIM1SIfSNWOHsdt7iCJKtwDhm%2F9D1oIS1M4o1wfq0NEFPDTQ2zchmpTbhdTLYIlUXUmRGGH7lefVDWeNzlkKTR7wwfPTb9XIGZpW9YsZXCHBxNCtu6t01eXG%2B8ErsIweo728pEvWCjnns4QO0Ahcp0TJuZ97Wu7qHhRz5vkKceNZBKfSogZrvn0NvzBdKJ5KXQsPAj%2FzTDapRwhG5ZyLynNOQ9XH9Uu351CFh9k5t%2FD74P9LCFUEwf756bVF0ekuLaRZwrWoQXlHeb7pe29MrX%2FXFmjhS4qx5fRIIZEmtDidAr48c6O0sOEAXqYmiPgNYMrahky89Ntjd9aEiqcR1Ukl5tMI%2Fq6OJrDyfQ95wAWucx0eUwUX07TaCT5dU27XmKGGA%2BM1qB3Dg6A3izhDOpADSeM7f9KmDx8wuUZggp3pFNUzb%2FFlU0rum4pV6LwSiBY7NvE7vqU8PFzWM6R6UuV2iFtAG7X%2FnL7SHBIDomcSAtgF%2BZDndU%2BZ5hIET7uo7TsAS1qIvfSVTfLf6e9m0%2BmvkDgK41bdsiJ2UFRQVvj%2F2cmM%2BHoSwnxAMdFipWqmyIeS%2FZGcKew%2BZqzjZ%2Fi2Bd825WA6KkTOnPEW3JJ3b7Qib3IgfZhEW3HCOzrWs9tFezzlQ6P64iwJWwBKJsw1O6cyAY6pgERvxuzapFn49cafC%2F3ZpXHGMLt0Ew6ehURfqNXUuw16u43EjEZuQltpxcBx2T25jj4nSSFoLxJ0REQv25cz6%2Bl0YRcoHd2xAizDsAI3esqz4IMVuaEGcmaEoas2NXeLGMsZ5RgbHEMqOr1fW2O%2F%2BNOJKimD5XCMrs4X0xYgXcpRpKRQlprC%2FhbLk7AnZUn%2FJEWg5jCMRKmTTzuRpJWYjQa2fUTWnsV&X-Amz-Signature=66993cbe0b3a289b54ed7638654c1449f03321aca95ffb60101b456587a8adca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
