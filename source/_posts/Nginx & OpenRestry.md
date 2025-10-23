---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3DWEBJ6%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCH3YxdrFXcWrOQCtEygDPiYqPwZ38z%2FKBupVf0Qg4XXICIQCdMfq9r%2FLlSZYcz0airm2OKgw279dRzEmtBJhYlzPm6yr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMM9E3iSJSjaaJq44aKtwDuiivSyEiwFQ2zyJzsnjJTMNSSOh4t%2F4nsyS0PotYAh4eGbvpeNFmBHJtN7kYDLOlY6CZPNm5AxMupGJAyawDmrYA9z5UD2EqUBoxeqM1%2FmrQat6gxn14BuoC%2BKvbG%2BKDFL%2BkGxkkvLRs%2BfZ%2Fxd%2BBwK%2B0YuGhtR5uesfu%2BYnoBlubMI%2BBmn3kSC2rj1YqhJgqO94jQLRuZhQjJEOkUj%2Bt4NhdXlKhFHLyrvTtubHpw0tcvvy4fXCUZhfJh35BaZ4sF1T8uj3RM6cgA5kS8zFdGKRneGNaOigyo2QzVKEO3ClD6dPpiaPkNW3o%2BszwkYu8t4BTS5KrQN9XUHIMRYuedWUje5ruVr%2FPEA2CsHW4pOmvMriNNAeU6LPyZugE7ABSE9XOpV2H0a4O0JTssZ%2BqShL9947N2RNn5uXbniC84WM5OLjC3QJ6BWMYrb54wVhQEe0jLmGKbgz510c%2B8cJFG8zK9aJ5qy1GNcgblHjjrwu%2B1TaLFEHKoaCLELJAgZBGLRwEXgjRgCdb0lNz%2Fl961ukXJhbPrfjXeK1si7jfXP8zfEHA8l0DWUrP41GsW08gSL13INjHgcDkNe5LSdygffHBOV5fuT1qccdH9szFIOy%2F6WHERUHSFCriDjcw2oTqxwY6pgErWsWYIJ4q9e6pA%2Bdnhv3Bsy8ZfxI8nKCkYYYnwevfnCd9SS2CbaGNS8mh1O%2Fi%2BTkuGUa6ycr0OQHMq9Ue3ePeyQm%2FkF8vJJHlxCdMsmjRq%2BfjZGBtqXTebKF1q6MxrP4fEIoVyyOCHrOn3BIb6fqzciv23Oi1DNvBYwV2xhxu8wlKeJ49BEn6gdvMpZ1JsFoo7giNFB3BuY%2FfCx1hvlW5B4%2FsiLj%2F&X-Amz-Signature=8e5bf2188044162f3a843462215078a094f8ae64a32f223e19dd1ad72f9e75ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
