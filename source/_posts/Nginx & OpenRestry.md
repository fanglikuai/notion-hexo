---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6EB4WIL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4%2FrzRlrV2QXhFQaAnGuLskVwU4ou12XneYHsf0H%2Bu9gIhAJdp7rWT3a6uKoe5H5zfdG5wn2p5Qu3VrQm56lhiJd0MKogECK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMkwBT%2BITA%2B1yESFoq3APk0SG4%2FxIlhKHALdATjEXR0RHPYilS6rilQF2uYRk%2B0ZWWMhB8AS1KKhPw7EyRNXqPvhXZy0YWpRx7IvbBsyOKVnnuXztea51y%2B3gBFvGaSbwnDa2WemCHlkm2GcQUoWaBa7eTOESLOQNogvIFPA6hAZZbQmC%2BvBhJGGyTkTng2PFKATuzA6C4rnwyeOIajmEsgRoNlWNL4uCkD4NuVgVHcV8GWchXWUR%2Bc82SyiDsfTtRsyGdsU5G5JLdUhNTk83ftBCgwGNqbt0EY0cgPRfd%2BlrnjmTMDw0sXG9mtSf6CrhnRmfr5yAOYN%2BdNa3w4xhIkxXpjvRrh%2BHtsEa%2FKwfY66rNlDXy6J2IIMN%2BLjP%2B%2B%2BJ%2FYR%2BYZrlpUKhvbiM7AJrEvxlVuFw3JE%2FQkDC0NgMu9AfNxjIkx%2B8E7XyA4dkubSQZO5UHF3BIj2zXdX5THDUnH1ZWctP%2FU57BjP1%2FEXeToBYhz4bgCJiM1yVwXStg4ac%2BP3MsS3gt1F7eRbjKt9MfYmY02jU%2BOjvCZxg8fFmAkfOiX%2BrYj8qhEp7MwhXogHSnlHY1mbG5CfM41KrKiWNv28QPeyQhd3N4easRV5kIWhNZZlDc8GWWWf5hRFqfzDlpzE699Xj1YeiIpjDbj%2BzIBjqkAXDYLmGHqTEbkr7vi4MBJxZhi3EHd1qaMPWRMULQFzUOJrDjDi9pGszzRNJxZUHijV8V4%2F5h%2FTp8bsULbtTCijwYW98tyObz5fmS6M3l8yVac23hao3QeHIkXDabM4MtjoNQok5QmRDSjMdnG0wjzr4sDcyx%2FufiWneIWTHP1Q2ap6GVUf1LYj8huVHQVcay0a4ZcmA9XlyTT6auS2pLOK1e6b87&X-Amz-Signature=6bf87490f409fc69c135326c2654dde3f329277d782c66281e1e6e8b2e7de89c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
