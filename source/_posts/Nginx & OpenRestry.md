---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSV6MYGC%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T085604Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDHEwDi%2BJbHCY8FhpsvcwCfrKkQWVGoDWOJOh9nPRse6AiEAsrNQkx4YBKlz1LDmlCJGtFr27WjpqB%2FfgOJ6vOd66Nwq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDNlPp9SpLmXHHjoVSyrcAxdqTdVruFbsjRIED8CdHw%2FqTEDxJo7sutGDnEY2Yi3wwvqYelQlGcrOSHg1%2FNCzHgYOl%2FFF8TyazkMjoVaCm4xDXJIqgeicM3hxxw6LQh%2BQH%2F9Qx40Q7%2BVw%2Bs4qdOmrF%2BrKM3%2BI4HW09ROlW8kkve6oy4g4ud%2BDnG8tqxbeReBeICGo984n7hFKH4A3tJJZUFgF3kefh4UgZRMhK1ts%2BAfsHDQ5q6c%2FOPuSjhQf7ySNdb3J9jEL7BCJvGYmkwMvX0vedHZsUVqznk4snfMvhjOU24pAgo0PXZhLGNANgAFsD9VedFzjrU2e0thZFIYoZx4NSohoD%2BrQXVc17xmycd0t16yOCwjb9UQObp6pm15nd48cZq8sgJDsqfYMFXDam6K1bPNQBfvDo%2BWPaTMnsXN9Z4Cu4JoInYMQxM82eAo6SOo5G%2BAE4vu%2BmLFIasqUnf4pJDmvjaiPVvTIWbQfrbr1WQYB69YWtRgdWbmPwf79OClZW8AmP9050a7tpoM%2BF%2FwwM984Y130Sdi4etMuIm69g0QOFiPQvhXihPNnMST0gp0G%2Fl%2FMyGAKwxod8%2B5Yp0NCkzCkkvVzXgyk1%2FnEQ7jsYEtkpWohczyPXwZqn%2BzICmFxW4JsWpCh0rcvMLWTn8YGOqUBpdavTBxjNR%2FohKL6fgbsGGlHilllFXyA35BgP9ce7Jrip6SE69hEb5D1EfVkiJ0Lih8pATFR1X8Rd02XHN%2FKEs78usgh4nQQJIsLVQVECnmJTLqJDewtPAQ8193UFbQt06YIlxos0hPNGY01NkMyyByoQY3QiFGWoPjLKLA3oXGhncDQy6s962vKXnatXkiL7VN0nCPvDznkUF1xvETPAfZ43AV6&X-Amz-Signature=a1fe44aeb3076614268240213f43cc110def52373de337590b61cfc52a2100c3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
