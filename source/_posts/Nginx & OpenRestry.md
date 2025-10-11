---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6BM5BMF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJGMEQCIEyJT36lv01J8j%2BSv%2BCJOvlXDjlLVpfjHN12GSMklF1KAiAGyPiDCFyOFuJ6uOrW8pZ8%2BSqTcl5Rsd9G9SXbXOEHOSr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIMFBcCh4FD%2BJJe4NbRKtwD8DPyc5nkqdhh3R4AdcY18iCkCA5cKT%2FsycscaECS7rGGr5f3r1W6RK52XneuGD1v%2BQAgQGPEpkuCbYZNCioL9f2%2FthaOTIt%2Fb24%2FrXv7hTX0tVysqC2t%2FjJRHvfg3o9FRhq%2FQ%2FYidoG1iceP2zYCsZ%2BxebWE1Sx6W%2Fz7Tb%2Bnep6yPJDIvPzUpTroTFD9VMDDFoDshCNThSqMojnq8gZLYZPSh4sgbWhPBi%2B9BIrGrtwq5kxFJqyu7k92R1HaN%2FD6me%2FibLPnHs1fW7BZ1AuVZo58bAqFJom27s0negst8F%2F3npKabgxQfRTvSS%2F34TpnmjxrfYSU9XMy4LDubV3AV9l51zTEg7Q%2FuN5BR0r0oLc82FXmJDICjNHA%2BR74TR7HALfvZL6EHhD0g6%2Bt12zQjFMi%2FJbPs0stc1hw%2B0QmZnc439izw2rcYjmibB29j5okE2WhDExiqaq749Y57YzUX2%2FTDdl2leMMDrCXQi6%2BOV%2BiDABVuHdl3p3n10a29oKf%2Br6qJ2gzJKmYNcMX5uaHr94kC%2FAPOMAYLBONPxS5WXR2cXcLKmlIwzEZw5czlShdkI2FVzFL5Pb61MjtfWhR%2BMjNjIJIZm2OrFEdqKudvVNsCwLkWH6Hg4Qd3l4wh8WqxwY6pgE7cNCp5WCKvxxXQj%2BtM9ek%2B3rlnKQ3njRcB2tlD3sjIFONlVCHrxhvHITFG9bzRSrkzC7xNwz4rRQU7y7a%2F341bnmmeAAZGNaou8h4cQTcR4ukh25on23UrnAuvGgz2hKLWcbxYp2KSmWdv0%2By0LR2ohoqCGXDH7N6pFiUam35nRl4YF4ppOB3jyDXEP4KJL7fB2nuf7tQbZA0pv%2BOSGQrqTuo5qqd&X-Amz-Signature=b58d79932a2665011e3a0654adde1745a34b863be67811f0ed9c06895bdb27e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
