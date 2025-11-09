---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667A56A6NF%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIEjbeiph9HRwisYM2XOx%2Fu7yhZ76Yv7ZPUb%2BsiGDsOHOAiEAuPceJSpcPGILanIdkvPW2wI6UlOfPGQWvOqqJVn8qhsqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNAa1qGy9Lf4jsz%2B9yrcA61OqRlQBpvJ8Ex9NZkHwT75pug%2BbCsHW%2FW7%2BwdMkBZE3d6JUz8mZGoHh2rbS0W585wXbkZPtWavVj2a9uPaf4UFerNaQqOAWqw43dU23ZJjXtDUSJCCgHWbDW6gCU2SxSDHVncLhO9HSQsGMqGuCSFHtU3TKp5qX4fNGjHzIkLHGWuvlsrsBmJCm4zSKPGov1kHo%2BiURu0VMIiOrOUE8dCHW6VpwgFALd72iEVrGw05sUb0GBj2lWVvq9ntRQFYc1b6fulrejL5aRT3V4IrG%2FX0ENXq1bKyg%2FcKIA%2FnDDV7wZV7J9eqHwMZBiZomA6Z0m23IZWJSXJ3s4TTPVpWk4HWSsgW6QO6WNvFz1rcN1Iyp97LZC7IduTiO1LdotdMW2LNpUt8ZgcMO1U8eUkG8qdYBKeM6Xhws1wlHgfQIVhmkp76ZBP16Zady3nJiTcjdbOw8jGGNIXZEtr18xrBQEPp9gL7hgmdaZp9XUehMCCNbTWsvyj9020lG7pte4%2Byz1OMkq9HpfXvID2w%2FhYB9rj3MDGNimFKB8xvfEv9sxs4cLcWRQoAqVjnOV0lh%2FlVIPdtM4C0dT6sozv2MxtCbFd%2B%2BOr9xyYM9GB5%2BTNnOwe5lrqxTMfYBTjb6J33MKaRwcgGOqUBPt0DsGRXLj04HU7JtqQPfbcda3twjwGzSkm73jfwK0VswK7DUMU5%2F6v0BLM84MRm33pMmhixozO2Cyg%2Bh9PqmUImtf08cae2oYSuZBMShCrT04aZKhTZjlWzjuOGkLGeP003BGYiLtl2h7Gu1S6%2BRdXZmEDGFZvYvnGhHEgz9aFjfSkPNzWwZ967p7aiFDlfwnt6SP70whn6HM%2BDG8gECyEWS5Tz&X-Amz-Signature=9abf9db8157f6b5398fab690ac199d6717b5ffe4b99161fe10a55e0681508596&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
