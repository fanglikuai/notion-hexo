---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKLWWLI5%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBGsc6slF%2FrrJLGoVeczsBQJvupiQhtkJjCN8v3DcEPNAiEAgCR%2BmxdbHGsgMoRO4HCxkA4xREDcdB6NBmbzWkGQ%2FG4qiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABt%2BrydxytPICmZfCrcA60AzxI5anlO2vP866r0JEXded6JqMTc5rpFx3XEmaFfEJm2zGwjpedjm%2B54hbVEkLlIyEVhWUMqB352ADFpadzszkoONcPVjS9mDIH4Fy3CCXs1ZC63E8ybbObSuEZl4%2F8UtzqbeWXdBbmfopN0lA9ktzECukqBnPm0U%2BxQMOiIji%2BBsApKYN2NiC3stE236Hueh5CFFCA3GOAummO5MgvkBF4TUQYGLrGZg6lJ6GlLou4vsaktzynbDqpALTqM0cEIQSs2N%2B9YLx8zcYafgOd39lLEJuFweF9a2F8CZJV8pKwFyutkI6MjVMHtI%2Bj%2BmANYamPcUF0FoJf1hp9OCZMQjWO9G%2Flx6l4O7dmv6VLpFo64YQ%2BUSdO5vPSR5u2E5ucn%2BlacK6kMu2QHd3yefaVLR1SGjqpKfbzwyeOJfKh8Yd8jIJF1gKAq%2Bre1rGfvzDiV%2FpSQvm0ZSEPVG3xz0UalNuza%2BcpSALSB9bUzqhVgVNr6LONuk4NKOX8J0A0QQokSwLoIyfOfmd4iAAdX4vzkXnd2zSxDZVqNMr5m1jUJaIsrZc%2FmoZANwImi4b4W9kJzEWC8a8GoL0l9%2FVCJrBTenqFAYHStXRIUsqn68kyY3CnLWeJb0OVr78NbMM3onckGOqUBkup%2F9Gf6GM41dkEgqt12oHEimcJy7l2ndevyx03gN3vtZ1tt4Ef1uw2xRkXrkTNsTTnhYL2ZsMwPaJRGaGs10qBeK2Od904467p2%2B9z6V6xqzQZvAaHT5AKa7r6INs7xbyIaAHjG52r5KoVolqClezkrI57MCjVoloBt8b78mPhUEbka8I6%2BjNX9ke1dt5fEOVVoJYR72xdJ0JjrXKusagnK0SBj&X-Amz-Signature=f3bc04058ea2c51656c412c9e78662fc9408891d58326a38e47cc6d4d09fcacf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
