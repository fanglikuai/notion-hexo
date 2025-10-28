---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYXFGVZR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD6x2vX0sMFFpt5o66BVB%2BYC%2BoNrJ%2Fj7bRza6LpK1qD2wIhAPcSNoH4ETGhWygXA2lNLcn8p2mjk1v8wsImYidAvWUiKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyjIiNwdrStUvDXdtsq3AOv2Mf%2Fygd%2FASWAwQPdyqnr23UigoaFJc2s3IcyU2eLPF7fhgjXvAL8Nwbz7ncCI7vusGBfpEWzLX2sGh3u7FO0XJeyTqbxcQsknnsTth74D5v%2FdQxlKtwi4lQN7vMvcswZkUoSaV07SSBZNlazvXj5eMUWJ2e4L%2FZ3OPnfUeES6twZqVvzCql5xPrpOxxAzKaZuYkbK72H3r3X6gHftLH6q%2BaNmP00s8%2FEDV7Q2mxjdLkI2WbUJXQjVgHYQpWrjtxw5XqMb%2BNGWHfQg7LVFslHyqhNbeCYYUYdyGWneQ1jQXnbWYgbaIGfT9%2FENU6P93X4nDQhRUGrJ3v7npr4FnfcZMwwo9B2n%2BD9F%2F8wBSySa7x2Y2PyX%2B%2BXwBamY5wGyDooHibnGY8kFYl61PgsqNLPbqGOXH5Spm%2BrwmlQVr4JWf7lgGBukWSZBnt%2Fd9XRjMzICwVwSnT7JouoPbXhba9Wz0dgtTZoB7wweXhzPGETxWmkSC7uyAaX0aiu5lyrb%2FUVSbJxJaymrEbk5%2BKrFp6R%2F%2FUUUN6onVLSPNiyXJNtgxXdqU8%2FSMO9b7iwSNXRI1QlAb6TlLytzIUKrFrzU%2FaIIOKzcE%2F%2Bu8EgO35owSywLSDJBwioz%2B4r2rqwFjChuYTIBjqkAR06bd%2B%2F3N9PVbe0vNk9QzrZTkIB8e8oLpKfvN7xsrb6wAFRHjnv5UdTcyF%2BkHd3NysE5NS1jlhwqCUvC9AGvS4xLnmDAY2J4qJLOYNJjR2%2FICgS70lXmrjPBrTeJ%2BsNnK6ggsg%2BISEfVF8M1wVTowBlpbjnW2PguK9CF0W%2FP5mfWYlXmyAMCTF%2BYpQX8XIgLSeYKq45wxCKvTIEXRBE6HWMIY4u&X-Amz-Signature=cd9a7cf7097068f0b2387e712db9047b78a55d7f45a5d1ee7b79457be8bd7819&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
