---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROXWW4ZL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQCg54wyZkn62eOKr8LQr4LtQknFo28tBDLxKibj%2FQoY6wIgfKBWrqJCeCD6k3llKGjmPaG1PN%2Fl%2BpVYTG8T9b9z2Dkq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDBOyxsY9qUpTFFYSnircA%2Fq%2BkfnWhwJf%2BStu27XdbVyNznN2udm7hPFiMY6xUwmD1V5jy5zDExro86OQg9e%2BLRLNNm440OdIAuvbZmuIr5%2FY0bbMv1BUBPQJLjLeyrTcQWNFC5bKHYdPeAAnRM34V3TAJVXIL4k3YGq9%2FVW%2BW4oRnlk5nT%2F9tBxm46SCVST%2B86TDyV0p1rDxkMyRooC0%2F9Hct2AweheKMb7Bk%2FJ91T%2B3M54aP4yM2x%2F6LOsfTs%2FXhexDW0unYPblcNX3TUWcH4pR2fONiP7conMMAwLkp5DyscprIMuS2%2FQrpMeBZMv%2B8gZZv7k4nCafa2z6z8U4d2O4lzJeujCVn7VzktOo2LHrcNa0cxzHplWTZQu2F6Dh9FSXeKZ4TrV1ynUntFEEMBjYttx0rBb33o6zBN2xnVhHOTQw%2Fd9IBnOEqYVcNXyAgfCNwXJlALhJ0SKDnAAG6smQAiPSlYY5BOZuvhfnL5%2BHKbscTL0oLW3A50tuNlEH9PAZbdVOtaRqbJYKOd4q3lunHa7L20SBDc0LoHGVQE%2F8qoOOe899lRTC6xz64Aavn9rNzUYy%2Fr7Tg9JxmLBielizqWYqnGevG7X2DxROEXHMKUowsC8cnHpSn9YY%2FDdBhW4W7zjOEqHH%2F4fgMIDQlsgGOqUB6FM8b8BCPgLe%2BtnLPNTbiyNG3jk1esn9ms58ukP1LIrh%2BzDkdcPbSwaUlUPpvUcldDXlnlbWwE1KZiOMAEidGUxSFqJ89hPkNu3HtXb5oMIURocCGUzuSJEFHCS0BsiH2HQCVMEU8iHXXQprV0vCqfiYRDmcmH%2BpvKUfPgnhwl6obg8WRfIHREmVsjOvXIYrmM2twF4j5d6gF7bZd0MMMb7Lv2mL&X-Amz-Signature=65ce80707b03ff2f9291e2776c4183390b1711f5d53b3334c7a428939a96f928&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
