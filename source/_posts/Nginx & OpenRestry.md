---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRBURS2X%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChsxTjNpzikR4Iw9gNoRx1UdBdGlmz1Y70HiPT3St%2FJgIgOMpoOYyB5C%2FXMKDVW6Pux%2B6dGlYscIAEJcNg9mkrt60qiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJhUmmRATyB5ZkmfYSrcA%2FvzVxZ94NcrmVjqs6htn84t7EPnIeYlPSC9294Uk%2BGFYJgf61UvRqSV7an09IYulH3oCQwpIYCSDuHzKL1xqttspaCeJWatOAI9xtIYgS9B9zV88DxrKrMcHflV2AHkNAeFNthqugGsM%2BDUn5qZZDQcBMFYjjJPI9cqtfN4CKKPy%2B1FZHvIoAbX5Yib%2BN9o5DoENy2oMQDnPwLHj3QLXYN9sXqdKQfkI83D4q1h%2Fnk3GFamu1NXt7ELoZ0UD%2BS3c7MK%2BLM4paYiy8yweKviKk09ayM2kLNQTtqxACJ7Vy34AQ16XcYBQzC86FwmrhYtCJ%2BhVqyn%2BFGYp8ke58JTQ0zW%2FksfOA49vu8JtY09rjarq3ZaUgR7Tb5w2MuQW777oqFeSMUmXTM9GNO8npxOZJ7CMKp2YTXvf2Nvf%2BTk922%2B3zYSHixXgFE5YjoelrUsNzJrh7r86ic7xlJkmO4OxxwsnFBGl%2Fui%2B68GRowzrNEUqx5%2FiJMX%2FxwmNvwD9dbabqR7ruFeVI7pDwvG6XNEniX15XGMSFFl6BYV8WichybzPP2LguTfZvw7lBHTJQtvkIHhBZZFkA2SMXVha5mGaC2kJ35ZWPi%2FcOlcAkXTK7vRrVMQCDW0T6r5Zee4MMX%2Fi8cGOqUBapsrN4N%2FaA0XNKLO7%2BlwSJUPyCbfl8purteK3%2Fx7RGT3AOtgaVNFjOf7qsaUtgwIJ5%2BIe60Q1aQ9tRZn8N1gNFUYaF8syc9iIV2gJoGaBM8SF6106Z0dg1ghFNHSJFDqdtMjTphye4IZo7c5LQSAUD3ej8I0awlaV78LnxhG3IACXp8yxPjOwmV6XRi7pexKZvnRDKC5v3aCajJJLXFEqXzbr2Vk&X-Amz-Signature=f3d5f482b5101474cb153c72f1161887ea7f558aa899c337a7f5a2bf9aa168e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
