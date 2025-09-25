---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QQOP3FUD%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCOugAc%2BPVDtqtU4yF3mrq2z%2F%2BkiB1fY%2F19kMlGTGgM6gIgE0cJx0bq6LXtvLIYxw64xtvAQlSOWDF1fVWiYOclvnUq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDA0BFuPaUWo%2BBTTE8ircAyWrIRB2QvwYqyJ1l3qZi4ENekcD7saIwFURewWKi7Nom%2Bo1tUA9syFUZhXps3qtuslWqKqHr5oqRX4tDnuwXveBQn3s8EiKNJUk%2BNJYomnLu2mA49BVYhzm9Fi2AGHAUOEEL%2FPSwY5ScK3jexVTPV98hlOhCFlRkyz%2Buoqht5MLH6rrUXgLAGxGlP8W%2FPEp2ubl7GHw0lAhi4doGxF1n38woSRLf%2BjXVSrOr8UVXcQWjUb60nDRuJ%2BILTB5aA67VgFa05Ztlrhz9x%2F78b4RM98jCJN43crE43YL5llkOqiB3K7F6jaqdD2QSWY43GH%2BWUbt9MVxkT%2Fun13nPraNjKCjHdtJQOse7mU92b1oNArWvUDF35TGM65EEDlFOSm%2Fk5FEcTeBYw8hTMvyu20ECDiaWACgz1%2FF0JZ8glMbTE6sFnuMhVquhDD%2FOJd74m7XQShfdh3eSQUF%2FP1xfxGlnSPJJCogbFKNWstaXCo1%2BfpQFNbTC54DjFsTWBjWjFZwfN5p1qOCe71EL2Nqb5uPDlkz9ESQulBwHPwSWMUGXX5piPel0fO31NU5oeDZs2DJZAdlHpxZspjYQi18xbCfFgahK8PqdOvsNwU5tKtRIF4edAYNuDgamqikKa6nMObo0cYGOqUB0iYhLKfMEGTd1mCTxPtE75rbfuETB523Q%2F9PoUfbbLY1C0jhJxC0RZQ0QzohqY9oYXaKiMZ3G3udR60JUkdPfxja9XbSlHI5Q8OBGJ2u9e397GzLlx0Vwp88BQcMTTSMcDwlYDT89llHY0tx6CaDDJR6xNHgRV3GPyzSJFyxIvZpNF5wSZ2T1vB39pvE2g3t%2Fg6ZWqnFFlCD1Ti922uEu4cr%2BM32&X-Amz-Signature=93070b31a3be9c6ea30d2d82c7d59b9e486700ed94f18d88d8e391111559b24f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
