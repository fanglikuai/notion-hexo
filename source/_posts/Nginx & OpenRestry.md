---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WS6RAHFU%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEueimI6nGAncmOozpDN7QLa2OGGc%2B%2FFQm1PC6RwxhwMAiAYSi3%2FhH7cvCw3L3%2Bzx2A%2BpyguoxK00OBtFfgG9DCe5iqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv0ITxOmPfuBuB2P4KtwDuWQcxhb7sBitMgkb0F0IsdE46Mby1cQBJCGz4LY4kpKBlHykIp6HTzbDR1kePjiKRrelG%2BH1x61eu0pfe6wAkGlAUeqW2kn0DxKInK1B5%2B5mDyKyOfnOumZjgS8oXBO2Gyw9RNla0AwmWLnGsaBHcxrfSNsuYVuMEe47Uy1H6Fb4SK298awUuSceCbJcAPDEVb6bZnBUIVMDJNoxXfpt0vwbaZ16Xbylv%2FvmI0%2BW6AVFWcy7tFVLbvXgkPNGGXj9xwV02WUvUmw9NjWt39bKD%2FcsnkrUUhgn1V6h4VR64yOa6K1K8Vg%2FvDXKTFnAhatGow5maBgaO6UcjdV1ts7dT%2F7umq75x5zcZjWmYn60ZnVHneVSan059ZghWq0356kHoZ5CPfc9%2FxbrNTsdv7vMnCRxGOHlq3ltwhaFyFdfw9z6v1xRwOP7bNYxO%2Fo3QgpFY2qYvYuSS3JOoDVSQPICYRiOjHQEUhZsQE8uhLb0NzrxiUmR%2FtRwNlsS%2BJ7JU7uXVjqgOKI80hy%2FCLjJEGgLhZ%2Bei75E0pyi1jl8BGWSJnKKpORcTyPnv6w2QsZup2ZNE2gU2uJoxB3LQbtfvDR9hoNKfg9haOQCgdHaD9ZXqoNmzPGSyIOO8DDp2Ncws9%2BKyAY6pgG%2FjXqwqX%2BO3IeMQ7Ba9mUne2tKIpWT52ZaeBxQliXjNPkyuzEZyWLCCC5L6q31Ul6dI3%2FNSUaUQsJmCtHAcbXZImRDrgq%2BG%2FKlv82TK0Rgmg8RlTwWnhrSJWDQJtIdBjBaUecxJ1cnZyEh9A9QGpCJ83ArRGwOwHHP6DlChayFdpi58wQp0P6yo%2FiO3WF6it%2FTdzPQDxKkwc6iGbmhJvV%2F0bmhhlvi&X-Amz-Signature=320e1589d847e304437d323f402b8759191dbbf1e781d9d9869a632a7d94b676&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
