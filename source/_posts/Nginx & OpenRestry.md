---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTNILZVK%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIG%2F%2FXWflu11w6A8gwyPfY%2FK0sFVShBKLqvCX0B1cObm4AiAFckh%2FYi1o6agtAkAoYd2kGyZFgn4tMxV%2BWwJqMwsMXyqIBAiw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeNt17MILK5JAlKtdKtwDI%2BlaXAphEcKFlTG%2BJACitQqJ9rjbOic8HPg8j%2BiNAxGKwFHZ0RpoZDKUjj0yllX8VpfQ6ggclbNT8il3y8BAb%2FHbut8TR5iWZa%2F4GDvC%2FQ9lw0P%2BJLET0pPeKFCYcA5fDDjB6wzykdqmkZPY3Ae4M%2F3mpRyVMSZ5yvQcd4DiDMZtz1NGhlgSe0zUiyPU9TUhbW1X%2BDq8jUJrAOEiI8u5kVrWIFQQ6oFQv4ruB5tkmfauhN6K7Q16k81JfazEsdcXmI813ahzqXXLVXGsIrByypzgkOcUsXkxLa6O0uIuXzw8Onou93UtVkgS0lrjHVeRLiKDixTuQPlU7HboeK5SgMe%2FURNUZp3bU0uD7POV0Hs9xJhZJVfEY53gGOG2G0aEEEWNcG592qX6n%2BWzsFNp2%2B5uzQ2nyjk1eM%2FeNMg0FLYEg7CQgumBiaRvCmGlrdDgxUhw7OcPeGHc5tlNoEp1mfXmFuvQvuIt5jBY%2B85N4uYckBXiIVKDyNA2%2F%2BP2eecZtCajKuq0HTGgLaNBnXUAukvf3KdKalKHJevqJfbE7X2sOENaC3Gfd8ouZ7e7Ajt8AP%2FHgxDXE%2BeJ6hmL9hw3UgRVFnHHRaD3FsP2xgF6PObewbnzRJoKg6yBStMwkZvLxwY6pgEAmZLpLj3V4hVNsDSuxnTE3JB6q8s6hL5hPMRB1lnk1ELEdIzFSeijTrmgKsplYVlMA5W%2BRw%2FXNzYkVJXRTP5ICL3E5l%2B24zpoYZtq%2B4WQGgWhClbrTeQDNl76xBfWQknI0gvUdhJrgbySuEQV32Ik5ASYSZHqUEDdVf5DI2ZyeHeuON%2BPU4F5w0gSP75KGYz4f9iEUK%2BxjHDXgnOztGCHWPjtal1D&X-Amz-Signature=9655f30a889d0d515ba6b27c3ba7eac9553a55daa51e0a66a770ead0a104e603&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
