---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCJ42O34%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQDQHPiImnpTtM%2Fg5Rc0VC2iWjdlggLm%2B9a9%2BtWQNQnZGwIhAOTEH7%2Ff3Z7A93smajIYtSbzcGtlmrH33pBOTnw5Hu8eKogECMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxEi7CHjElk3OqZ2Coq3AOHQPG8hUXKKfVLoZznjv5udelciUObwkYrK%2F6ZKVMFwCuFpMHyBshfqOxlIbIOPyxMv8xuherENTDhD8RqF8Lmq0BT%2Bczx%2BljseFZZ7t1DSbh9TQpQhKtYqhxr6Yy1QQR7ffrsn0yhZazciO3rwVHu8EprtI0m0ixqAOj3GOHS4AvCMJKZfUSEB%2FGY9PEOwAEl7f0DGQjWEGHbRibz4qaERsxHcCfrJLUSalJhFEITI%2FhGrLY6r%2B49JT3djJ9VfFXF%2F8eZhjUwt%2BRUR1A5fuGzTPqlaYfSf4R9Q%2BxlNdfyUwdw39Ln7VxQ79j60vdvMOfCsxG6vChjz1OzHHC3XCuGX5d6VXVPa8705k%2Bg746PRFKy5tXArseHAZybuDzjpUQT5DtVZAaorOdKkM8I%2BOCwLzAP2ZzCBQQFLzOPBmmKWeDK732ZaulhlHzrTwdvst93qGvht4xnuEMPmQIikXttFkX0qXcF5RNJqC1wplT%2BRL3K5%2BgxKn6XnD7c0A3Oe1f0hofk0WhRz8XO35fpLG9YXh4DLjMQCgS2nouHz98deERuL%2BVsv5qKSqYYrgJFUbJt%2FJ6l9yJ%2FrZq3V0%2BcUraJpuvT8Ce0Lf8vFWAWF1mxRcnxATeX9%2BCzky%2BtljClvJrHBjqkAZP1BqK59%2F4QtRtnTd2DMHCfTmXd4Xoe4zsFtyK1wvN9moYLaAMhwVsKDRdoWPENHA9b1TSWSHzjZSnddgUeiFdVVZT9QPJCBerkxpuZfrfDVqqBvf33FyTKSRFMxZrdoNGmQFZCseXIeFJfaiaTBzpW1gdbRS1VWESf%2Fz6ZjFeZwiMS4VlugI9lIPhyfmuMhpR8sbgH0tJ2m%2FS0P%2FOFXKJa%2FUcD&X-Amz-Signature=9ac2ca160bc91cbdebe085ac5d700a128be45b19f05eb7c6d2078fdfcc4b3ed7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
