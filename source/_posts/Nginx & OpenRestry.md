---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QOO4YDR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCW8%2BSDHK6VPW%2BTt7c2hDkqvsxEZgJotc3cT22%2BfzvMrAIga6MJqOqHfyo0JvoiaaHSNuhlJXPtWrExbpI7X7PeP5Uq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDPmrXfMer22NRGpmByrcAxM74MabOKtFANVti6xJeCcko57amjW%2BoRh0IpvzVvcQAbaSSKfa%2F%2BrmvZf2hPKdG%2Bx240R%2FZ57QcpZg4RZOEgAZOMDGDv1MT7PETdBkmo5LLKU7lipxiJuqMfY49RsjPA1UCSLtszO%2BpuLUNX7keUjdh3VvnXKgBBtVsUHtloqTTg1ZVgkvAxsDh0Elj0eyVGPkI3O%2Byd%2BjLU3COfGGBLy09%2FIOiJFEUuEq2K2zErUpsmyGaF8OKtXfbw%2B%2F6elqKrJawuJSWpwl4C4R88ba0Od6NcdQVu%2BcC9evgLZuPAge0gFPfXhhCDaDCIzYDyutGB2ysDtuba9PPI%2F4wh5DEImMe%2Bmu7F%2B3MPeT5mWEYqd3dQJMFcYeJt7Kd4iNJaIc7WgEkM5Fv4FTOdBX851B%2FAv1afnKC5muA1TqJ%2B48f0KOxYsznKLrGxSebo4pEN0nD9lIHTbre59t57bBcg%2BVmERcvmn9Bo0eYRQfecd5Uf%2FjVfSIs9%2FYdH%2FUgwi4MLx%2Bt8tlU2Y0kEyZ51E7Wn%2FrLVdlKrBLyRGLdxDru5fI8fVjFFAqxv7YJ4Mi7Mw5cx46ZooTv2fZ21YfPjt%2Fc%2BiOnMY9lPHPKaQhs7wE3go8kF2aeMB5ru7o8Pk%2FOEOBMMm65ccGOqUBLbcCtN0%2F0%2BUa5HzZWbFJJbtXbcf4ahhda4VZy%2BiGjAzu7XqDU2w4QpUmVdmlPVAHP09RKzPIvw%2Fj5w3ZzJJqFl30Aq6MAer44SyOwslvoSRtrHcSmCoS8utbLk1HvlZAGTf43GVRM3VAd8GBENdT%2FGHjflF%2Bv%2BRC48Zrr0l5QGUYAhzJwtkRcWfS2TKqRA%2FBTxNewons2OB5jSpEPM2dXzI4esw7&X-Amz-Signature=cde13ba5e005ceac6a8b7afd8d101a0b97e70c31ceedb620fe8001ef2b21c479&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
