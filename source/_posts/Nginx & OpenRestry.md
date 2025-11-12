---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DEXLHFS%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQDDSAU%2FwzFjxt5S%2BV%2FcexH21%2BYb41jq2Aokh6MT7pqpEAIhAPCII%2Fzktvn9pDp7RQInHGUve7kGBaim7HUFMdRTwvnUKv8DCCgQABoMNjM3NDIzMTgzODA1Igx%2F%2FWavk%2B27W3YsRqoq3AO9mP%2B%2B9vm7HGshMvsR3AHQD3B4xQHbl8thFdUyiuX16yqM3CEcnZGWFcabnZhVzXa4cU4H%2FHwrXpQ4wdKhjIB8a0oIdPMzxR1BnXLNpdD0%2BtA7ZlovodbjA4MPZcua7APUSxYiqRgVgWubZWvhM8V34lQjUm5XZlp%2B5vSkaDU2CVUtHukMXCf0SrqL1v7ELksSyt3nMOVKtCMyMf3twvJlsdw3V5pkpbhJEjrHnqHOd9xtp9a8nrEkZAcOKYY8yuTQCpkf2sN1weJk1UWavPBSIG4KkNch7FQRiUCFfK%2BsCb3hVDnFGwm9JbEKL9QrMhdA5QfagW9wUdHd9MHwRjHgy7TjeKHp8S77AweE9G9mnnsJh7y1Z%2FZ8V1kDB5%2BomfuZ7tZiSWjwITIgUVAiKp%2Bks4nsHKKi26QTWA3OBbDFNCutuIr1Z%2B92QPUO408YAy3uq8ckaoptYGDAomEUfnsxbyqXpx1JRu83xpbtelbqO9lzzikLYKMsTGg%2F6Y2xQGFgjQT3iqQn2AYknqnl8PaYvVoE%2BkHsyex%2BmkJPA%2FYdOEoC2UhxxWf8XiYieWEGjD2szky0nv0%2BC0pwBjAGTbxa%2BIYycttzNnEjkKDVmNd03572i2pwy7xY85jZ9jDsiM%2FIBjqkAYyUKK3fgRVy1qg6CixkIL564gfp4UsDmUff4fQA6JiiG6X07EgDBFqIjgdwe9M0YsO0zV8hflthbqZ%2FG8Z0b%2FUWGsdKGjqNLVMVOG3xErwaAig6FyXsJcnv7%2BWZh1sn5ylpqIu%2BUdhLL4P0RCD6FhPj1DN7ueaSRy5XhpVFlC3H4jSIHikGwqDq656BHlx2B7cgGEal72dHOLNfNcAIM2Y8JTK0&X-Amz-Signature=f1d49bc52d7ad45a0d3c8a030804afc6b0a75d63421b3e12028029af21f0ebd9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
