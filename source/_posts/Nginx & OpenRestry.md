---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7R3A6QW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T170105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHjlkqMi7xefwZ6yvzuRh2oBvnpyP%2Bdj3YVbsQp7QFVAIgXW2Vd93eZdx%2B%2B5SjlJOpapndpAzc%2FGEztxIszFb6ssUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkmGqOm7aVt%2BQFASSrcA2odl6D%2FMxxfYLDwZBTvHs%2F7PZRnfeoOKhDCgT8656vygDSP8qII2%2F%2FOMIqxO8Kfcn3gvxm6T5hfyziEpY12ZRGKeScm6pQ6ttsndzOIWI5TKVGFjcnP11HUFFe%2BVlLMjRGwdCamh%2FvOS0gpe3lB87HHYq4xrFazAGFZJbwCgP8uEMhjESYsFuI%2BYU852MPs2tAQqoONxDG4Yod41X%2FkIhfzxmVeHOhQFU7n7y6KVHBf%2FM2Vzw4aFv7O6P6NUYoFZ57spEeUlo81nips42zGogfHU8gPquf3mQALeXaUkWnZwU4kVkgM7d3MfPqU9tgqPlU%2Bwao2x4ajmSDXtfcdNv9dRoh2IJSbvnKZSjkjphCspX2mjl%2BgTt07Wug15rIwTwRdThe20iDggIpt0%2Bu3eT%2FzLac1aNEnRbHBWvWHbFiHq6ioO6x3%2F87UpAd5uiFjHb%2FWcczpGIJ8uNtMTcVKyw8Keb%2Fs3nHX1BagAuA8L8sViL3ulp10BCgTDVWbidAiWnMwWr%2BziemeHeqaUrOlgaWcAUaU2ECzg9qdJqhl3dXdvivX7PuWTn%2FgXvANwA8I3b5QeY1pQQinK1GGOPflt6j%2BqLSraHF5MGCzMWkNHWuresBjQHL5%2BK%2B9rLFxMJfAn8cGOqUBJhaknO0I610B9jMv7IeUzN1lntUB2qHD39OXdvwlQBb1EdmvMikUd0CTUgfLAdvxPdcXda64jDyne%2FBArAiC0tDFAn5nx3fmSRvCDGYhWQnNcZyFyOMhBbJaNFUe67DAhJVBlQMtTwGLqaRiLeT5mrXaTo0vtg4CXfQeMGKxzxrzAxptF2npj9z%2FnhJiD3T30%2BYup4wmGk4dX%2Fc0csQoQqmYFopN&X-Amz-Signature=576b2602a9d059d3680d1803a20bd07f24d2fb30f130938dfd5fcb84dedaabcd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
