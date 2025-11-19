---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCRFLFO%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBRIIQN6r%2FSqng%2BbQU7F1XWnpdNTgki7w5SmUZ0fkfLBAiEApSEbwK99x8TkDi%2BjGI53apkmrMed782EaQKTzMUzYNcqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPO6SBBIKl3luQs3WircA3%2BCRJzZ%2F8Ta21EFWFPieJV0jlR47fvQzh9iY5kxDNM5i548O0EerwizVuLrpohiU3miwebhh258u21nKi0rtDWYLov4EXMAMxL0KYY7edL4wclBCjQPS0lZ6Bmh8Dkwy7gcyVmTS%2B8LY88dNHPMPeubNW0%2BlZhJwAiXoV%2BX%2B2iM%2Fl8twynfP%2BtsJ4P%2BddugwngwCyqLW9G%2BMZKB30hzrALIaaBT76aav4wooDe2IiePHepK66FzQvvSPt67zF8ilUg022X16Tlb6c5Vpj2vFsZFYNy%2BJ4wvdiW7A%2B1au%2Fj5JJPqJyFWLcLLtzjhSLF7c%2BBzljNOuC1lq3y9WvMhcqL45r0OfOwKlCY62nAhHItjThpXwIjhRWl%2BjTD8AzX7Utkx2FtcuSfzagakRU5Krm1nCxrqF1907n9eaIrBxg6XQHN82pGL7lf%2Fn1Sg%2FpT2qaBXPQrelrj4JVcKS9LAbPBmAJ6YKijIOmr51HKbghZCQDf7NQzReiGBa3zEV7yB6uOo%2B5rTJE4uaGnyRX%2FIS%2FMB6GbC%2BrIuejKSmRX0ODDlrjIh2%2By1O80tNvwjulR08MMKCqBwh6dBZLbgKY4iIPDPOwVKOiThnxPD6IqJbq1bEa%2BtL%2Fw%2BYewxoXzTMOqU98gGOqUBx6v4OLFHWHiTwsH3mwDjKJkkFQchRzzGP5pDoixg12SJkF1ZdXaIhRPVY6tRBU9TLUn7d%2Boh6C2i9cylzrjzUIYLIIIddiLR2eNxKy8p%2BuqYnKMwJryI44IalXqWjsrE58ysPa5LShhUl6C3dhxM%2B%2Ba0NjTW8XPVJBeK8qHF0srN8CMPjxSqMSf%2FdYzWDIJE3lMWJh8uOBsqBxtB9AOBEa91IL5I&X-Amz-Signature=5aec3ae68d4c334e94602795e7f1c43617db47b08021f7dbb64cb611dfc6c899&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
