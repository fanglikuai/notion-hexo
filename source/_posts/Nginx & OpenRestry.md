---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTW3GOK%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFGbMXy%2B5N52mxIjIEj%2BvENK79zhVGpm2e3ulzOONvTgAiBTCk1XmcfagjB96cJbL6py20SAFueBMXThN3MVEHl6Cir%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMWYAZMMBmpP3O%2FwpyKtwDGrb367D6h4V7rpYyaN4795Or7exJ%2BwdH8v54yfCMZ8%2F7gkpahj22f4lhzlkRPoRIAlRTqAgWMZTw9eLLD5dUJoXLF%2FWJM4UMgNhLSeVtpXaTYFVU2D2Anwdq4ImID8PKPFK1I4JznxpwfffRUp8LX6JEINTp%2B6kFiI75RgN1B62HGEGMjCgKYbt6vtGeUY5dYN2lPS%2FNEjz2eFJKgOc1p47%2FTtP8FaGA4N%2B%2BftEup56der0Ih%2FFEGSlhU89Q5IHwX7D%2FCa8cog4DliJXB77ulECliQwPSGVHfu4Vq28uirPIYOkw3bKAfnmjaLlOOCdatEgyu67bUhAgA6uBt9NDVnKAAPTxfsaxk%2BHMMJhqQaChXq0WNk9%2B582%2BMBHPfDs2iuWKQ9zFGRkKL1NZdAEPk%2BGG6Keu1IW9RCPO%2FcKShlRboMC3GTNvSWOh4RxUzHUfWv5uC9fPVdDOC80CEBlNP0rsaX1rYez0idCOJxKu2Re0bBHgWoAhHNMuyCxZET5qFn8DOuPtBqs34X0Nx15eChn%2B3ibWjwz5TuBTfmJa0myHyxOskz0yG%2BjbHe0ycuPXnmpGBtTO8ckb5JDxJMtRgvEbncFk4yYH30jht7zXyTpVroVstZxWbO9B4P4wr%2FrvxwY6pgFcjzedLpzloWV4C%2F2z24YeBRoHq1GoCgRzbLybh9Eu%2F%2Fy4brU%2BgUpdaIo%2BD8s5l%2Fd2bPq7FwBFx8K44NTLR9Nu%2FBCH90HSkMOeSd8fxxcp%2BIf3J5v9p68ZpHhh1qBjxfd6gSArVZ2FwoBtGCVnbTFS6eWthNELkS1uD%2BURfr2VzBs8ZVzK2A5JazGcHkkEdY4QSYPia0lZBD94rbwRMwQVrs6XP3YQ&X-Amz-Signature=060fcc77746041fb89889e0783704ea12118f8c1ccf9c9d2c35fe56214fca400&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
