---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XUQ3BX5X%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF0wehRpAn7OS1ipa1%2FOceWuOYZb7OWDm5anxdwHgW2xAiBycxTGcmpod8%2BfSu3150sK8pE8UnXFhlduOgfCyU77PiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyxKOjNlYDp2A4w0vKtwDN4l7m9BUSQ%2Bxp7qttcARBnFYqKprKaqcIxgxMJjGfIqsgAEdlz09iQsywZ8LcR1nwbTMJcA%2BvCy2%2Fq%2FXfIiRC32FWogsIakxPqS4YrQuDGxSayKPmwNAE7KwJMjWiH4mkB5BLgHKg4fhFjUVuqmP9FMp1g0BCChoG14cWZxh8CZHom%2BbKH0g3TkuLfMM7yUeZSkKo0%2Ff6Q3MxArirbZ7GaTedZLTADMlEUTz1xzUDAhpOZU6EAZfuVD4OjaWvWlIQqfXtZVhAKxY8pLQijFQvCgbhIrJiwR3ZRagHuQB8ZiV6ZthPcaKD3VDbeTbJsnyqf7NuFM6zsl4%2FC7%2FBVVfmwAJZBEPhye6rZ37DqApGOsVh2g24KwjoFFBvNg4K90ATsQAfJLzehc%2BxH7dfKUXykL5wxQBc9r716GMG%2FYl2qT2yaAKwYFb3gfI6RlnSGEz4v6uLDBo44gSMZV3rZwNZb2qmU508hzKx1a1pZMimcnaaJfkFbP3HdT96BSiQ5n0kJqifgIK7mP145PbvCQhlsy2MUdGna%2F6cIzk2tbNZ%2BYiUDNUuY487Af92nI0LevA64JvdEevyfyH39Of1H1Ejr%2Bz7YcyA5Op0L%2FHfmnPwzEEUbIhGvDfieZH5Zgw79y1yAY6pgH8efQK9pdPoVackfSIEq4uR4bErKd1JQxgHqkpOn6qmCwb2H8k6S%2Bdj0zmA8sHWLlrZ5xd%2F5Ka0X9iPUhV0jsGMH8%2F5We29AdMUsJz2Vbpz0zUThSvXJ%2F9rrApizg6tAYrPkdWYz7%2FQdJnIokm337DXqAVhVblKsrLLO6uHLbnFulq8SU03GR8twWXKVscF%2FLcX9ceCyK2oi8HtXFS5egc8FJ%2BmDZA&X-Amz-Signature=340a664d7d2f1f75bd1ce493fd7bbad8f23aa9566974c846133bed6047dd569f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
