---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZO2CUOA4%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHJu5cj5OBVlFabyfvKtaoOK9w7qlxgXQc7ZaVMq2a4gIgVN%2BKJU%2FTj%2FqDfY6LQeiZO9aB%2FIAQUabembcukuYfjHgq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDKC99MSsjZQGk4ODHCrcA5DYTTsrfGjjRzN4UzogvYEa43Nko0C4H%2FcqaXjGdu5J%2FNo9Ex1aUoyzK%2FWM2kLNYVYV0%2FuFwRu9Z2m5dXr397X%2BMpxf8w7%2Br8nR%2BVbjOgWaBcp6%2FsufSAD%2FlcMceTHmhPfJ4sRGfwUVkqEbYPlJv%2FxeSfrxUKDwbmr3DqIiUGPT%2B0WiMJnHZzB4OyVB7%2FVoHEVdMysiXoUVBbWE0k3E%2Bj%2FZUwb0oGi2ztBcKilYd4Y7vZ4Hi%2BvJ7VkWErnGlbHRtIUyS1Bty4IwMFnPbDJjyxU%2FiVd4WSdASK8jTIVaACWJYBdPF%2FjMcDookOhQ%2FFzy0Ks641bL4Ih4O3te17zZvlzO7qFrlBBZHfb8wun3MJjTXMOV0cC0M9%2FBN%2B%2BSHWza3LzbTqNjuq2BQEU%2FoSFDHXfB%2FGkI8WV6Fz0PVOq%2B7YsnemcRm2I51%2Bm0%2B2o7wqSB4lXcmJvrS8GT7iGZ5O633ERm8hvuaJGbiMTq0qEIQL5t2mMVYfuW0ypYl4HabH7i06uAmxsEk1aQ%2BqeiPbytYY2P4YQcvqhM6QB%2BT8cqc7%2FO087TSVVpsPuYQt5H850onfrGBmmLMcPZ5YHmFJRXKJJST3iXVvuPhHMK8H2Mfxpj0MYSlmgfqDQNoK%2FLMIeGkckGOqUBNIyoapo8XJaCmpZy6FPulxg8y%2BZb8goS4oUwqbs%2BwEPl5Xltv1nHTVq7hKRXUqhESgtk3OorL7HCfiqXLt%2BJLjADyMExK5QBerIBJ5%2FDmuzg3uQVUG%2BBCEZ0vP8pFvNZEIKHLckCd%2F4mEi6mgw8YUhgdakIvSdjnvw2laQsWh5rqEJrU9Ep6auLANjaCxdHeGatk%2BacpX0z6WD%2FnUCuIJaACseMQ&X-Amz-Signature=240cc2867daa7ddd2daa41058ed87eafbef3d8d63642c4bae03bbb719e762010&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
