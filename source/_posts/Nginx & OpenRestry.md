---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662H4T5T7E%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T090059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSTmXwilsqV1d9Cgl3gKJO3eKVIab9stTWwIkNE0YqxgIhANQ9FTMEEZVkrR%2F94ueiv7OHkmBGjH5lPFmLZxDko%2Fo2KogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2BbzV2HJ8lmtSK6Wwq3ANoMukOErDlSgFvt%2Br%2FaryCd7NnR9Tg9ThtBW6WpJKi%2BgU6FD%2BMPHUOXP0vHU28hEI%2FiveC2O7UMcIriNgBzUPw6l%2FthwINZ0Gf1c9Jm80aKlSnHo9xcw5eBcVD%2F%2FLf7Jy8cyaPxIEIZAi%2F%2Fu2rlzALZyoG%2FD7EgcEZKnJJfyU4pjfnHgps0870cNETOYD7HqMAmjyUEDWQZ1%2BwmN2KtMSxXaxMLYemdV%2FdyLPTP%2BCUY3WHFB%2BJxYqIiM2Ojg2g7PHzArBBNjV0jLXFsY0q%2BYfPMKZpD3kB6LjQaM8Ek2Gb7pzybHr4CLraWr67BTbLo9rXWoFZgCDyYK06VrCF42kFFWC4S5hfqThIYb1RBd812leC4xzmFSmTiiIWAQ6hqIDWC7n4OM3Dv%2FfV%2BtSelknlEQlcJc7PZ%2F6X0qx5rml6Fninn3xPtDIb0LQar88%2BuM%2B6SfUcyMI4LA0%2BUIQCXjrvhxHu2dnxSQD1eFDdmJuWcDqBroinA1CpckBN949dnZMGNKUXOFCFO8UFRxOC4%2B7OKdpjfXXPGoo9BpqlNgJlNGiRf3pWiePQBiOx3nyyvcjLqFZUHdtysnRilgMcnQKgTDWO5JdQ2ZqY1CcVV3yeCDwpZLb9qpZCtgF1oDCY%2B7XIBjqkAXuc6KuDGWuggrh2zoYst7%2FzpN9YkdONCbwBHSwrGhKNj6Q42AISlxCbr3j%2FjyTylElrWUghHS9tqaTIJDHgoBlWyF1zytPx1KpXRQ4nwUTVhc0%2BFEBzN3GiNGYkxaI6CO5ABJzFqLba6HSc3ubLhnsU7k2SJ2S0CR7uMEhwUG8IXgjMTs1I3rwK6uVBgoXJht2JOcyKrfS7TYO1ZU%2Fri5y3lwie&X-Amz-Signature=935d27d559b3d5d075d57e2b5a94274de712ae92200ed3a3c6c69ed0dc33e715&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
