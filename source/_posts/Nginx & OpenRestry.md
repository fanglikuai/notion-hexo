---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBRHIGWU%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDLNgICYSerIY3a7huxLKz1V2B554oLp0DF5cort9%2Fd%2BAIhANNzIT%2Ftbid75QQqlbD40ct5VUp22yxEunxbqQP0qUfLKv8DCEwQABoMNjM3NDIzMTgzODA1IgxSa3HSjE6%2FCErX4uEq3APLjmHZ4I%2BZZ7QKlcjBdaSzfuUv8yqpwCqjzln97JQhvkM4XrnLjerlst06JrDDDZjolB4JdI7VA9%2Fz83naCWv1PeTr%2BhJP5bKO9qR7mznL4OXAxYIq6e%2BUwH5evDkTWNOiQ2aqftR1M13rfErkE9yJ3EUcyXmpZaqeQBOfXL2oLdOKjJp4jJPilx6fPmZyEPrBjnviAfo6zkGvL1X99hQiErL6n%2FplyNNHxEvHT4gwTMxRnETZhfOWMHIK2eR42Gv%2F2WSPLuaktkb34shpGFnC%2Fqy52EXaFtjD9AjqfNvtEwvhsbvH7b40AUolfRdRgm3kgq%2Bh0sN%2BrS33W%2Bq06FGFsyj5Xn0Hj4mO2Ddj04Lfz7v1D%2BmS%2B3kzrfsPoLKdRCTV6C%2FH4jG26mk5nqrjtn4CN9Kot9QjtCK7g8vk%2FPNDtjzEp1j1S0kCf9ktbtoNe7T%2BaB0D6DM2%2BCNIFInLNxtSKp3aswf%2F89FzeoXUX%2FyqnMKw0qy4AXNOYSFo%2BOKkGrRWL8stXmKwBmQAU6AIMV3Sciow%2BX36CTXWwPLat7TreYTzwY3Oc7lzlNisGJKldsfTveAkX71n3iGJgw2F786IhU6zb8ZYIFetM7nMODEiEPK6UQyahSFm7zIZlzC3kLXHBjqkAfyKjG5RO3jTAfn0be00yuy4cF%2FFB78DrEtv4AMjdsJPB%2FWn5MPkn8PN2MGKSNYbGQhXrDzU5oB4LrnqxLJ%2B9v2ASwKJ5o1tM0hrcMgoGSkeHaFerLBgterqVGtOIh8VX%2BZDteSIpnP1GodwzljIrYa18cFYk4ROey6SLVDUv2LXTYsmJTREYdkO7Cqn9dHmx4fqSx0fYj1xuMf16RpOJlIs1dIJ&X-Amz-Signature=e9168b15b48d103cca94e387e9acd5bdfd554a77d877be22ffd178deafdcbea5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
