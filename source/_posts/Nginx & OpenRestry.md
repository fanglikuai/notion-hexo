---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662LQHG2MT%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIB8CWHCGCI69Q%2Bzz0WYAQXHtKaL6yFyZsJCEf%2F8GGemKAiEA%2Bb2booxqFsq%2BERexx0kLEeMI2hoH3IE7wndF9e9uWfUqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBwCp2hRnCkI8AEhjCrcA87R6WYyxAKPs6kAAgwndSLq0%2BToP4WQggU5%2BR3wygAFiNoZ24TCHMW1xaChOzkrsil5l4565FNlzdpwKSNQYOxJGE1xd7NLEW7h%2Fb3BdJZp18Eu4y2QTeHNLKMch%2BXx%2FTcoELda3%2FMbu3c%2FvBgw44Ic%2BesczllJ4yXUaGAy97%2F9QOQWO7OMtxJbamrdmaE%2Bi0GoVWBi%2BSSslnNESDCXbdW5pqOBu4TnUWUUiAFdxn41tucuzYuvTNbBUegyT7k4rq9tIk2xHYEiijK0RC3uhqf8mDn%2F%2Fin6nZssM4Zrsj3yUSYokevRBLr0Lrx0cg24Q7IpaL7uzm6mZCdW5YftVXv6uZoWgm5a0Mb8cHwVuMhvTuUWSaWDtoc3NrS5XVzsZJtf3A3WUaP3re%2F53o86AOzQXEfIgli7Ujk91PiQSn07Vpkzyrum6a%2BhphYDvLX0FltVfmDTVwgdLiY5gmp7HW4CxYQ9LM9ev8oFpSFXbeUQl4rAArhRWOtMX9H3xbDR4sGhf4jsAft3ap7T24KlJGRkDTHk4uxIjEoiJaVQdQMu4OOIrr5%2FefGdrsInAUj2B6QwjgimY8C94z7YIzXSWNUDc1k%2BMwURgdSX7nRHrW36hRsiDhuZKXtoZlmVMIPj3sYGOqUBSBg46bNE2TeJOLLA%2B6V8qUe9p7a8YBacjkt3d5u439dfSBAVJN8GDoNHF%2FTcP2XXXKV6NKkkO8G%2FHAf7Un58hY2tvPNFiiEuEy8UgEhqCA%2BiS69Sw2eHwNj%2BDl5kX3A6KGvasDQG%2Bs80MYa03NuhgQZk6uBGySgbt5QP0W187K6yW4JxmEJqt5bGwRAyUzDziLXiE5ASW8heUtIyf0la7lk%2FjEKh&X-Amz-Signature=b2296ec6e04f3976a80208f225e09baeca259ad6dcb6a92d2adb91af50c337e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
