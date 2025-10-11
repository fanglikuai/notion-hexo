---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KORLGAU%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T040051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJGMEQCICLV9sjuVDGRECIwfIrWHHsR1lfUa5B551%2BETihFkL28AiBcurPLWel4osT0AszrNJu9I%2BFZH7Uwro2tVdNOIgytDSqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7soKfVXiQ5MrL4JxKtwDazwQ0pQ%2BIoEcj9jIHV5bF1Ex5cjjTb5xbh%2F%2F2d0yX9EZuNVpcEoNt1KORrNnT9O%2BpVlTfosROAABTLeAm7coJ1Y2S5fIbrUAk3ShXziEJsJtsfUJvnbKtLd0MvC1yw6T2%2BqZ1Ry3aDFnuyLUwR4OtYlua2dpPOwS1HPr%2FA9KKcj0eOGjtxob7e3Fih4rpK4bI5YD5%2BCvFqtJA0psc0FuRvikbdDaJJ%2BCAkJGRxVopiPKj4GGjrFfO9WrZTOvIUQkrVvX9fNo4qRWpOmvD4sxYwjyHACuJ2ZQ8bidWxDeyuQTJ5%2FRml9FW%2B8J%2FHaiMQ1fpH%2FBEWVZ6XpK72B1tMwmQLW6I%2FHBHztUUMu73bY2FxJFq3%2F73h2fX8NZvY0TqXb5Kuy0PzIZ24ZC9aIJY04PeKrrJt1%2F2xG9Uz1cgXtoUEVPiCspdJIB0B4p1S82zKrx04hWxhygAVYBLHzeob36D%2F%2FG2yBRgbvGIL2mWsiZwvUsMnb6tK6ytzVJS%2FPk6gN55BEeiVZYnPRbpZ9WRSgbcZ%2FqdWOWEZwamkzSfmNDrPa7Wa%2BcxLpGDamo1C9m51aTIDLU%2B8v1ZvGisbPmud11Ra4WeWONvOpTYPUht6xwXxJpXad5WOsm3IdeUnIw3qOnxwY6pgHMvKEtkW5XzWDhoaZu5myRHJ3KWgPZSkdFkvTvcbUA1tJhaVDTM3jgD3OLRtUUjTlSZbk0q96P2IBbAZRwTlt7rfODinDLdyGwoDO1UGsDJapxkSbRtnBsdO%2BMbjV6Hlqprs5spEmdX0LhKmc2CR5w%2BLvna2bWR%2F8C1Dgvy8LPrhtW%2BP%2FKsx9uUnVKadDNolSzdu%2BDWksvBamzbP6Bnxa%2FE%2F%2FIXVds&X-Amz-Signature=514e5b814a2f2666357779764049e7d1cb69f0e408495e54b277795120c4afc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
