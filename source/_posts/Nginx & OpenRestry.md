---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZOCLPEK%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T010056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQCxeRPUmbkty5RIZ8qn1pJpvbZPnKV%2FGuhHJ%2FBOLPdSVAIgA30eFYkmJIsK4C1gzojsVQRAUCncsNEJ7oXMXX43r3IqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNGoKYgQM9HLCU6kFyrcA2rYd0GbGlSk5xHPxhwgeMaxtSDT63Y9ken9mLDyyURlha%2Fs40EwXk2WYnBUj1GRsvfd7xp7APKz%2Be1llAZHMXq%2FAU3lbIR%2B6ZWsHkP0tY6diDo3bT0fHu08nUf4Xe0%2FBOLpiCxvmISuIEyursfFhgLaNAeCM4ZUxid4%2BVvlhYjehoE3Pk2L3rJgAz1ZoGvw3U1ZTDfKC7cJQ0kOJAbVAr69n10gkTJUyfvbYkK17bsAQTMVHijzsKCoC%2Fa1IrVrRlnKVBZ2jAYMtKfY%2FpjF%2FazFFprZniQbNSMzacFe%2FhwoOx6Hh%2B0Cxxq15TanvyFj00jeNQ4GZsGLJg42wRtxn5UBKRPh6K68now5x4XD6188zNRuhTt%2BtQiEEYrnxVZtbzrGuJlEEIFnyoF%2Bg0O148uOwi9Pt9ubr%2BzXIcj5cIJBeoZSJVitRKf4NqnmeGHXUkr8ra%2FJ4Tlp98858EN9ip8N%2F1KOdhrs7y3uQ5VYBUquc7k5sP%2BJbO1PqOMrF1Dip%2BIOGg6IREDJY2faRqsNzqk3LVsZFSy1XO9Qy%2FAIsl1SiD%2F2azi5AQU0VSwbUiE8YJr%2Fwn%2BGV91ISBa5Jjx7TYCkUQW8X4gzsCSWACG7pdMi6FVbj0oDNJ1WjHVnMKz41ccGOqUBg6hOJlDTP3CXzAtzQaHud5qp8aVy6cEP3i9yJDfjsNhUzdlDDxuuw0z%2FdOFOwPlL2u5uBWZfHKFPIeJP3XlmeJbIWmXG%2FVYl7Bn2B3BJ5VWc87dcHEYEGsqYVJC7KixEMQCLy84QWA4fKib3%2Fbn4uAOwA2NDt0%2FuvBvo%2FplEAhcNaw5q%2BlW3cpOs5bNB%2FBepkaG1YyYSI4%2FqqC1vIOmz8t4dBFlT&X-Amz-Signature=fc11da275379a386b1f1db817423cb29a931815d41c6e889d60f3cd95de93497&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
