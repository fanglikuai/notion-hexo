---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRKQVSGT%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHD9LzrOvtwcHQbZS%2BostJxLiRkTNpf3mIxA8VySOOU7AiEAgP4ywo7a8VTJ0lOwgEYbr2eLeZcfnb4FKbzmozyUeyIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK5TNSUCYhmeEKFkWSrcA%2BpzANLiYp%2B2hMa5SbNHHrn4AaG2PDFdkkDl54E90Cfb5oNaCmwGe4f6HsGsC9kK7KTg9t%2Fif2VA7U8LuSGoqN2xYsVKVZbIOPJ5zERwR3%2F9A01GJpeo1TIO5MxKgHl22WL6G%2FNsiFTdvs5xFBSTEzsKqjITwk2whI%2FkvNIYw3Qr97wqIu28EKF%2B81kOdgTG2BR%2FysVHU8eoGyCq%2BICoqFeNmRGzP3U3BYisVz9QLA1iWKTqZxuFDy5duAIVV8LJciUm5zTBbcSFDQqOcDFcCn2MmQyxexVo%2FOnfdKRE2ldjp8HpR28KARlugmaze37bvOh13Ow0gFtPF%2FOZYBFYC04vRnlNJWUQWPOkm3jZzQdGcMg5LLS00%2BoOanAI1LIsMqkm%2FnWuwj6AD9dJTrhq9HmWXYX0Mb3fSeBMCwHrYtFxOXJpFWAuNBNQtVdORB%2FCMVNK%2FMQdz%2FbYfE9q%2FbUcjZCaxG39%2BZzyaIlHsclex3gvlnJbgk1%2F3hVJoJZFxXWBGosb6c5ZNHC%2BitXwboBvhz1jVbVcWpkWp0dXQPzFYkc9thU%2BnCvVaKxfC2wqHzH21EVUd%2Fc3%2BpEn8fiWOFRa7HVxw5F4gF6UVlFZcaqgFuixqp5dHcZKxAErg52JMMP95cgGOqUBdmSweKo9JGsYjC7dlT9tfMg9F7MUGmRTfPBPAEse21F%2BovEh7zjSUkfmOevKLAD9zeJiYKLFbgCgL9HGGwEmartGe3IomeYq9EIQLH4tcIGuScKzOwQyOQMJ%2FJmzLFKKXjjT07sddjorIgIlE9rMsmmESkuBcvHafAwHkCpNcGl%2F6EyEMsDOAzvo3HUrWylx%2BSDXpOR2%2BpfaYga%2FHufQZv8pdNY8&X-Amz-Signature=480da3b6f5261f8fda6a56f6ddf4828eca6e2f8598e0d4327d1105655e464e5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
