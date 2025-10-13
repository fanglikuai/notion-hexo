---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSOLUKZX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9xpGBJwolkcupLCxomIPfb%2FR1tyB6W0rKxjN%2FfxdbBwIgaVHNTI5fi6WodAbmJydfCUh%2F0h1DGd%2Bp%2BO7yS5TGO%2FYq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDAUexFlmW5CfJ7x48SrcA09P7sSXOXz8tZ%2FfjWYG63%2Bdnm8lpWTIjk%2Bo0P3DegjDpSmJ%2FohIinnWHtnsJaoBRV3WMKoZodGYwcKk5g6M5xk%2B4XscH7VNvM9VYgSTlvfkzDEINdrpYzpFv1QhvZ0GdH4sRSqtDlDNj7tmeGfGJxIhVyuBeTVoSTas3yCSM2J0FJJCNOhliriCtd7PZgb1%2FSHdI0nfF%2BGbFtezY91dk8SWy2ng76qeqDeusLmwM3QaIrq%2Bkaql88vZc2JNMitZMs5%2BS5YqQ75EehH%2BztjzXjCp5rXpjTO0PZV5P7EqBkfNl2LfVmpWZ7YV3DdVPi7EJYG5K9ST1NkqdYPjjgjyfHtQxPLuEUwlQ2XcZnrzSXVpJFO8UhwFIkZBXslVm7vy7rVkdgZG5%2FzhbMGRqzlSehc3QQxvv7ADa2AxolRgcHghqWxvo9fw15VMLmkOJNN%2ByoAXaCb1l393LOU1Iac4yH5V9c%2B1%2B4gnCyiKxYmONGsrDwB7ze5SJTaAFgjZgPqQ0Tj8YsIvyUs43%2FFP75lfzw6g1K5hjFgxXW%2FMnR%2FyMTyLz9reZCy0c9CUeP8Iq1BD6AeihKxjCZ6v4VVq8%2BAMn62Lv%2BT%2B9yF6vSAVTkGLSlQ5lEMR%2B%2FGVRiyPmR8GMO3Ps8cGOqUBRdV%2F%2F2mwX4OFJrAnORxLCkAz8wX0urpBlXEmQhkOOA%2FdBqZVtPflH4khxV%2FYe89rJeIQiQesw6%2FtFFdPi2NMZWUUUQiksdamvaBUNtxMIRgZcWgSj6FRES7eoyGb1AE74sSz8S2Vls4KNhydvWpa6Tcr7qDpsOn0g9Ix%2BPQeZunuzdTBMnjg%2F1Yb5CJBBjDzxveOiQvyeLuOMu4JnojmqtsnbXlD&X-Amz-Signature=2a41fbf2aef257f9b5048960327a219522ecae99258ef42e4cf9569aa17c0c11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
