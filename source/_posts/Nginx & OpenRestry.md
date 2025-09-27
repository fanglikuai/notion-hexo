---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667XEPYMW%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIGtn6Duf%2F%2BhySN9tue7wr1S4iAnfHLDcOjcUntVM6y36AiEAk7F3kyIwSIzswaO7SYjIDJl4J57Znf%2FdqeVHjkpXl1wqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL0upvA8ATrUqBOFHircAypEzIQSSSEXY2o08esADAjnneJoImaM7jM%2FlegjrY2B%2BZT%2BVZnY4gtDlBtIHS11kgYL42EGTvW76BQvAJf2PL3bGQMqJElPDTYxx6BPG3ZCWvjqrxtnmolzRCa2X2HG0BCZ%2Fp7PZ89BZO99%2BgDZHGQBhmJzlGf0%2B3bg8PS5uXUffxB5taoxPHoYpjlXesoK1USpSod5q9x3RTqEcIkkbaJLW1ca86m%2B8rm%2BLUg04nchMRpNKOT0D46UABlbhPv9tn72rZc2Jz9n2D7kQLwlRnrr1m2uEgcFZWwCgXC2yKQ2yANEkXnfR5Ap%2FG%2B3vchbNbbGxhl%2BjcMVv11XALHzUp%2FXgF0e83BlWlVdFsb3YMf3KqZnbNCVsQr%2FaTsa2HQKlNOX6guv9A%2FFCsgl0YzhBXKqDNxBR20obhaS62RV8gmXJXxqqGMBrZLN3Sn8hOg%2FBs74v1j2p%2Fn5VMOhZ3fbAqiZgGJPD6eO73bs24yebeFuhcrloKH%2FzeQ7pHdYCt5sNCRmZ%2BYhcgilMPDwismo%2FZ%2B57Wxlp6FcJ09CNwNm0plfYFQ%2FuZRoU2MQf8TuIdyLvXWzxppybV%2BpjQXsLgFVxy9mRpcEgSBU2a%2Bk%2BcHkiocbCPq5J69A%2FiAqWkWCMPjn4MYGOqUBB9gkrxnq1%2BHzZYQhzE9lq220%2FyS7LxmiyqyVsAkYDDph3aBZg%2BfXtsN8M4ML2KF2aGUZ3u12E0wfNs9paZv2JS7v5XHi%2F2Qj6NMg9RKfWfjHntzk2b2zLfJmzu4fwkB9wq%2F0A%2BF40%2FPIV3I9UJub8VsjIFNa3a5PRv1t8TbhtN9w9Iy8rIoPdWGY88ty%2FAWio7Ie5svGOLCocSUVTiZSzwSogpz2&X-Amz-Signature=eff3157eb4dac38a6ff0f53381d4da0eec664cbe3974373e35122470085155b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
