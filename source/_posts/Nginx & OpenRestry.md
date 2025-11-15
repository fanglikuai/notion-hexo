---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FAESRK4%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T150049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFI%2F6t2vPg5RqOq2c7QNXI6EPLGVonXCUB42Z6U1dqCIAiAIop6GkAl6PWkVUO9C5SGCUuhLK6umOCVTqetRZc5I3yqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxTU1m1CDtWP1mFkRKtwD8tUP8Pq7FYbhm31vtkaH9Qs4NWEN7kXFT4uK35iIy4VC%2FBnc1PhK3u7pOMReE%2F1nFccngEMocMhDoSM%2Bhx54heQxmDajerGjH0WOaVWtlhRoN5e6%2BIloXCvOP2bdLFYkoNecIqv5Fx8GLwOOXRVkiF9WLYIQbGrkq8%2F6UR1MdHNArHn7ek3k%2FfhqMz%2Fs%2FJoov7TUJm0yop3a1Bg2mSgpgh1LsuIvDHxqReC2SvxBqDSwxOzosIJWeipZ7X08VZyYKdE58979iMJ%2BABHez6%2BqzUlI865SId6q3feTHv9Jv8otkC%2B6AzqH9%2B%2BMcln%2BImc0SReNFZIJD6wywLf4bTCoDdv6U9wqKNIlJ6PA4yE5ziSXJJlj7XhTcqZfSGicswyX0ORjvj%2BVywHGlKh46o6eLcC0%2BAYNmD37okTUh9Iu3WaQo7WFkJh4uZZJYEgcjwEBuexeedHFeBH1X%2B5tvn0mKAOVpCuiQYPhDTf6n%2FkssfbdHZMmmb0xuEmjEMkSAx4zxHWXVR8pdamr8V20Oyc5mPk%2F%2BiO%2FixRtrsBvepgHjiHAeuFg3ZV%2BAaJBFEptGnaSTxrq7Hpg3QnDDFvFCayeFdm9v%2FiAVcLVd2629SUvkn%2BA%2F9ckbfN5Tlzzdrgwm6LiyAY6pgESYmSTzsDQKXSRI%2FesuG7sNTdPrrqena18pCN%2BqjB0OdBRjjfOt3oRQVIQ%2F%2FRmlzmTHYlUlr8P8gKo%2Bgs0Sy5t%2FbyuPdEXnNdB501rlNAByQRw3Ly1lC86DAAY39Y1VGzCjri52T3fLm96YMpfv%2Bhm2aZjOzJNEHJSsOl8GCDG9JUcOlZoyJcGCVIvKWsS1vO8jALnAzLE1RJg5UP%2B1tRW1gDLtT8J&X-Amz-Signature=7acf8130614459fa4d4b4f525c5f5357f7ff4ba58ff9c0a96d4b7ab6908dbc64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
