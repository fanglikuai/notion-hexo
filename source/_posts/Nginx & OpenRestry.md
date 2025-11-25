---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHA6TGSC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD46kQ5uY3ylUtJnEBoc9Lm0Jp3KldXtF8vp7M17nB8BgIgSaRcK6y5RIRJ7nHvLEH%2FosgLb5qy%2BaVsrrLDn2YQtv4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDBivjQGysjdy%2BOneqircA6thQ%2FElPG51NAZmregjhn2T6D%2Bl2gYpYFy1ri1yAYAkgLlKaMzl%2FIpWOPF6HoJNc9kb1qdASNBFdMl%2BOzJv9oK6iZebk7Jx7VyuoaBpiTHknc6hjfDSoMaLW489oMaEkefg99hZc6eZgH2qVfcLqsAJYceTVWIVOvKkhHN92wSEUF3IpzYYrfHr1NZzBpMHTLLw5dZiK7XCtHueKH4Q0BuwSOMHDZ0dc5zd5Tqi6lRGtwfabnjNWQpYeaItK8bQ1J3ZjInr1rosmCCViX9c7vr283GOhZjyEFUIAyX6ILMcQ3eVs8DlRrSEJXanDcU2milNaWkjzJ4Wgh7gyWjCztaSId3xmh5LoySBhTlSU%2FL36QNLL1W1sDmgO8W%2BJxeIviO0wNr6JlccZkoFsp%2BMnvA5yfJjg8K8pDmus2tL9HWmUo7M5mQxu8ZEa760VX4FGF649rHqJ0gEzbD0Xoer3aHkttDh7D6CN%2FEcvwEqJhMqUdrsw5lHsDFkaYwlTg9iI8BAigvLYqoxRFUdONlOAO9qNn0YrXdKzlrNR7IbWnxBsJ0izWepaPIia2uUHNS50IUsCzoRU7BRh3tSFLWTuEQuuQ0lNl7bdF2ZQtbHkjSDFK%2FJgCAeGJ4hYxuwMMmClckGOqUB82GqkG4uHppGvOFPclh%2F0KOxu0kaLMEu6n0ifnu4PJZgcOA%2BuLfBuqIbbHmvT2DB9HXfgC2AzqIdGKmaWkw9YUgifk6mIykPlioXXPI0G17Q0NFJlDVbTWaaH4wljpGEECHJAb2U8D4qdHu4jAIhHYOBtIhg4Ps7txQragsVhCMw1kl%2BTfNE%2FwQ2nT6wiSHJmiZECFWdDDBiNFL0ONBYsHQM4iT7&X-Amz-Signature=0a0396db3351ab9b6b4279a2f2d0b2e710ebcc53c76438b878953baaffd42aa6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
