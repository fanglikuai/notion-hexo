---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TZLKEHNP%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIEqeLDzorVmaXEyIWhDbWPcnRUDl9zlAXQ5hsky9rqi6AiAUPvfrjrSOAj%2FJ7KctaOkFIw0Fo0gaH6oUM2RKH%2Fa7XSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXVkGRrT5R%2Bygr4NUKtwDO%2BoRoqv7TcV8R2Y8f4LZI6yOMdjpVkPJ4Lp3atREszBc01ee4wX9%2B5oKC1VGqVSdmufN0b2TaGYCveWFizjkztTsVftP%2BYbLKWxfnypAtdAvbcOl5934e5x%2FzxshSaK03NxyCVRjz4EsY8S7Ve7uVUKCU3yU7X26RthFkkMY4FWJRQ7ST%2FEaOSpYBUG6S1RhxezB9Ro2R64PNdBkFlqy0ZrAn6t1vZ01jWtPKpekh6Koxeq8cN8jOW%2B8gdXjsZZJhZzoAUnvqU1uSVo5erW1Ts7aHp9X8wffHcmLDUm5yb2xZEImbt5OMh1%2FSaNnR3rk5M8MMhHZrVzgC%2Fl6DrbrcTacGOToJ1WYzmW12lVCz2wJiWYSRu99Zb%2BkJQNxIBV3ug%2BsU8b0NIPWdHTdGK%2Bxk2MIOEnCm%2Bj3ytedk69zJV0lmkZ2aPGv4iejrQx8cRxvXiwVUtanOpwXyn%2B6erRhk82wSabIKmF2nsRpjxiFpGC2d5He3Bk9aH0c%2Fya6vuC%2BSDctwjZKc26zCbaFvUnxgmjXmrvl%2BdVolR5kfDx2aYC%2Fdl7DqgWRyPhIJFm3DGPgCi4XWDd0uPBGefTz90sc4Ls65BJC2BXGBbbLa0QP3MiKL08jI3c6UDkkNOsw7d%2B9yAY6pgHtG%2BKwzthI3pd8i9oBm3mCIosyUqbhXc1DXm0cpbZhC18B3AJ73AF8mqTVk6HjsGpHeAug4nb5W2VzbiIAvvT7vysMYPkExBvByhsSmJDltKT3X5TLkoHVMQUF8tOK8ydtdU%2FSQvRcgo2zY0CC93KY4SBan%2Fk4CHx6fxBNJcLQML9FbhKT581iM2u7vxFklb84cCNncD95Li%2BBXJaC7UulFi4ixuL8&X-Amz-Signature=55b90ba868e72ce2197144edb2d69ff48e968649e42712597149a9481c6f83ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
