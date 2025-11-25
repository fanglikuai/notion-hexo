---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4IZ3IF%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAyZmNJ%2Bwqo7Cd6GhEvI8EvGb7ZOXV8wcDyPxzIRXdw4AiEAuLMy8CcJl0JOZJjo20YL6%2Fa8u5owmdA1QZdLfVNT%2Flkq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDPjo6%2BJQCFVbzh2qECrcA874dUElUaeKjCAqNv%2FCTZgOGIzvtr52VHavvrsylsgNRmnvcWxiQBfHahOKShD1Zljdp9eqVlS%2F5lNtdmuShOa7%2F7yq92bmJm3Zwn3ndX1VIng8Nw6Kd5IrYuZCSY5Z9lx6Jp1qrMRjbvekmTBfGW2ZJRHla%2B4nXdV32Jyb9alD3lG1mjTlh3wWNL%2FSiyIFEOa933CSnU%2BZ9HreTgfbfvlFR3xW%2BgY4F4JwqTCy0wZvphs33NUvDTX%2B6gx5zut8BqgCTLnHe1ksaQTtu1SNbsFy4cbjI3hsCNyKFeU7hlRYkOJmfcVfEoD2ve8NyVHPLI5Hm%2F7b44xbNIEHqdS5uC89klLmJEP7mcXO3iSzLAUcK9DYh6u9EWMiRQZ0L8aFQFvFg3TaSAQ2HtCOV39MMgOyK89Z59PkrDf0Rfldu8wkLs%2Bsl8XB5hVhqZQy6nTi8Rt6I48Q%2FOfaY1frKIuZps2iuXL2pVrBPyP1uAYS52CZy%2F0CxMzg%2BV%2Br%2BkYvjMDy3nOC143%2Fh%2Bh%2BbvuWGQ%2BsDLwfCNKThYQfPB6XjBtJCNRX1BkaJnj1yhXxG0qMpCSsHI00tqqMyfYqWREHT%2Fiubi8uOzKFHgazDIOv52Gqr3Ug84Tc1DiEB83jXJmJMOe5l8kGOqUB%2BVLMgN5dkXNOHWPdhV7s1WZXEXZk789%2BlQTScohonItLtwTR4%2F06Kwr%2Fpey5y8iGoB1FQjs0g9qlEofmhBmGCtl0AzeqfmyqJ9BUZI3Q2J61x4YkI2TOxmVHFzmPUtPzBxRvEby445xOxSja5XpnPhwH8nEA%2F3aC0THp8Hz4HF2DS6O9dZOIRMBDO3fFiuniXBoCwR%2FFWSaJdcWWvtNrMggpydGr&X-Amz-Signature=3c18b4f8badeeaf689d96121cd85e2f3f4bfbb0619d16530b218d4b16456205e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
