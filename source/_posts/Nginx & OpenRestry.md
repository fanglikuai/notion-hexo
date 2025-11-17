---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BNLV7OE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDUFQY%2BIn0k%2Fmjxuk2SKMjQFDYLM%2Bb9KRJ%2Bu56gxOwdcAIgHwC24J6g4VOp8aZIuIdDbfZmZmaopDumRoWv8TSOBj8qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLUtvWVOX1xCZjOrXyrcA%2FxqJRec5ih%2Fmxv5Fu5VmocCfTmmevh1WTjHywNm2ipClw%2BP3TSinTQCWIfJ3ZuB4FQ74koihpBFJDQDJ0EiCBOxjbhiSoOhTwNgVSLXMLbw4Hgk7GbFzHrpP1NR%2Bi3pz%2FllBcujA8X59zS1L5FOOW%2FOfAyA%2FWxrcPn%2FrRHxVpmeHPq4H5yTt4kd46WsmEQ5zyoWd9m2bpXAq%2B1OrS4IZCBEAyedoNyJnjhHbfKIJ7E2ipp7YispXd32DWY0%2FbZvhHUSNcXjN8PcObrvP6StSJkwNwkQOfTe8palnLRyA3Rjn5TjaCf3pYBhVVF9i%2Br1I%2FSLazMIwMP8rPmZnILhUozrBqdKc0zuh8JBNjDJ4Yy7N2CLNvbIp2vdbHAGXrOv%2Fj9xODpGLKAfzzmWDaA0K9VicBw1kQzKMxQtE2PkV4ipZE60aHeAbfq5r6sEAOSbaEd%2Fpki6ZMWfIIithX6Nix5MHuZ4rl%2FUtegYRZw7v7u2iELAOf7dwa6EvHrI1UeI2t2pTfmaUsXqI%2BTfoZey2jDRfLuZCfTLcTiUOQ0VOwzaj067Tvzm%2F%2FrIRaorKe6xmCTP7GySHpS0QqY9UHtUDORg5R2w0qaHNjOQ%2Fn0MY3xoIZSrJeeQ7b3soSrLMNmQ6sgGOqUBW5ZHiQ8a6MsLy3XLj0Ulvvk2psdKnG0g8MQG0v5L6y8gXY67lIc%2FkUP3h9HexusSnJp0VpjoLK%2BMeD8JSAjkl%2BOGc7%2FOH9pts2KegFQDwvq26ETvpupwFGOMwuVhSeczeIbQ0VhFHbfFTnIEl%2F3ZG%2BXckLczaiCh2ibF7m70B4F14HJIK841ZtKK577d0C1guoOTapU8800N8MfaC6wyscx5duxW&X-Amz-Signature=cdae6ffd5cc7741d3dfef028f9969985ef3d8194ff768d4f82bd73e94a3261e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
