---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCXQHK6%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T090043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDpO4EJcgDOOtPIu81raIBT3lRwRuDvd5MLieP%2FS%2FexngIgEZKBAv2XYS5uelzI09w1GQ%2F%2BIJI6t%2FAEeZBK%2FIt9n9cq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDFNmb%2FnYs%2Bpn4d198CrcA7IQZdO2cVdBlSpMjrzKjwrhQHorohfyef02zxsHLV2j4LWILRRZO8i3%2F72uQ731DZ7d%2BHsjuz80ndNc7gBy11v8TJLKpqeRJAQskwiT1LygnxCs2ugWThLzbOgoVMYTUSOlteNSxMUajn2BQbKpKd9QBXd%2Bz53yEvCVtZMuUfJAMcRxYrEl%2F8MwI2EmO9SmonUzv7OwvzIePSFMjOOEYMqgGBvJOILCLXwFSKLeUj%2F%2B%2FWmPayq1K8ErDi%2BTfe9CO3UvKcRZtzNKvMPtjP0xw4Bo6oMDsrDOtuNc9bwtrBv9I5GZlpWGMwv%2BAQzlL8aOfJ18tk0egGm0QHLk%2BEXmcWEXn9iRtK6OGaM8oAIE6wpKiBsHSx56aZmVs6vKIWsToaO78QHaJttx6mKL9w5079oDAYmDzXxsJPkAukf9v9I8nGLgWiEmB8kbJRCmmftEt9eWF5haGKzi9upLczrfEwlg1%2BkNLjViyb3Syjjgil7op1DYDG58YRIxtOa8cLtyqvbAT%2Bpyjo82w4Bb%2BbVTMhHcuOqrH8YR3bb31o2ZNTJs46Dr4uvrrdlc7KA%2FPysvRfvDT%2Bek5Q2P8jYtMRfuufEE1SIZid8nkswltxAsclEPWQGGFxYmKS%2Bbzp%2F8MI7u%2BMYGOqUB7ecPulxFyZtYwy%2BHPlTZ7YzcR%2FOEHYT8MfvkeeN7gpKSiexmx7xu0xVJkJAUwmYR%2FJNMhI7hxY1GZ5BnG7uR5g%2FACZoHSmec9QNJ53Z3KFlQ6QLfg8luhFTC6IYWG%2BxHgVLhvQzKatgz1tESgYvtbdhITM7qDY0fec2lvbazS0cQB1G7%2F1HyuQ6cmA6fdXB3LKFMzZMaDfWzIOMQiFAL3PLyhkC%2F&X-Amz-Signature=59844d637626caf5fd92d6d195ab65de39538267e00fe0da94a02f3258679c3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
