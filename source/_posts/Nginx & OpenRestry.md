---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYUGVQNN%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJIMEYCIQDDjFEY4kgPfKYqSKDUGI5Y3RH%2BcxLL4Iut3rTAjOOFvgIhAIvPro0DdpvfdvK16OUZz6igDQq%2FIHSZD4sId5l0YOVZKogECKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRE0iyccaLgimw%2Fgcq3ANNd04pcxLtp%2FLW%2FvkVjJ6ETF3QZOYIpr08gC6SYA2%2B4bj0a7MyuPWCME8vIniLwidbCZZYpWpPi6DRewouV5SAHjN46e6qa45moVTdmVSJQLOHYw6P8HoY4tO2vMoqGDsb8lM2RT%2FEVMAKQylJa5jU7zHhJqk%2B4OboUosIvCCkIEB1ft%2FbFYodSHNTu0adKWf%2Fh0lDkoCSSyD9OkxpQMeQ7vGbQPxrRL31cvMhJTWvceUIct9jifN%2FjEvgXnJ4L7fUWZpFKo2JUzej1cvvofen7nIXliqVV2UwXsvr9i7W%2BXiRSKcWZ29allRe6ENWHbuIlgkHJsnhPUjPSA95dngbR7W%2FRsLj%2BGcKKpjcr26fdYzsuJ3hpS2UTjL8FiP6rMMIqaaCz6COVL0UmI%2B6MacZ%2BAFvVlK6H8eJVCLOD5hzj2mbiYr0yIk18Ey1B3mdB5LcXkCkajN72NfLGIvhgEHROGoBLhZlUE9oyltAtiKeO5IAiTz0j%2Bvh%2BNfMOCjEfE%2BzloTExdw%2B7Oilxqf6V6PIMrdknzzW9OBsrgPicIwv81fI1x2oQJQAFeqpNV%2BjGIGbsFcO3zWC2MOA0IhFgZdXUfY%2FzMBFUCMDGB01mml0umohUUEbScaT00mG5TCg5%2BDGBjqkAYhWjcpM3RrvdFgV1%2B%2BmzCI7dU3KWayZEJv%2F7q6uJ9CCyc%2FW6G03UgpEsyZsWTGByAz%2B9OMV%2B0jaRD6C5ZCCktGrNJnp8U2cYEEkBESz6%2BLbxhdl5XLHE7%2FLY3%2F1rrrQvP3KWTnclXf0sW3kGSccXbeOTjiJxWQiVoClcw1kRQl3oJxHzRLe6EKstfKf3nwDaP3NBv125HjzL4CVYXij9g2tGkOc&X-Amz-Signature=4ec60cddb65cea30dcb6355552210f5d92e6fbe7ac2c1e80ca495941a794d6bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
