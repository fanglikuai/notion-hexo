---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OGITHNK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIANyA1yRJ68WUNt1LMap5G48bmFgLkjmgsWGed5BvEm9AiBQZNcr0fSi2ADmHzuJFEflSgBSAcnqzKXPWKQ1hCLktyqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhI6rjEV4dND0qVu4KtwDLM88VCkqavVtBWKHF7KW38w6HTDJ1R4rGw1IWPn060G9NjuH4aFTiBI386zDE4m5w6uDdmUYOM2TAPfOpxcRG83ImjDozD1g1t%2BJbbb%2F07Jk16IUtD7X1nLmoOcqBhH0UaT7mJbXk6oh7I3e2Qe4lyYd%2FUzTu2FgYGIL8NA%2BDOP8uTooMEJof9gvVBL6pgXQifc8DmyqRS6LApJsRuaiukC5phPr1MNa7JGH9FF2x%2Bd7qZRmAEERQRrZWIiEI6%2B8W5ec8Cw%2FX0wMUBGMu7Onf0c%2BHpQM3JxrjP%2Fd9%2FBby0WTZO%2FpDdfE9knEGIfrPw8Y%2Bk2UTRt7Tf4ctD89wI7IgqIvX21ZaWf75lSWMQI5IZUAKx5gfcc%2Fv7U95XiJT610oVsNXe28vGVhnlnYxkyZe1QVZSsgpEtQeaE%2FIEYXJovHSPhDrqgNRghAcNxjQUiWHRleoJlnmbCVKPlmH6OnKaYX4usbxPBpEjs488LxBLIC3HjfaDpEfNLhIIOff9bTK3RHZB3tAkb2G6k0MxrJz7JQBkmBnK8NxWXVuz9OZjJatlJjiz4OunWjI8jUhQ9luY5DGPJ%2F6umT3EQ0vHU6OTAW5xJA2En3wUoE%2Fmv1Pdz3o2GfjDfOkboTLHUwnZb2yAY6pgECTIC3WaiwPD5HLb3NWrGrRo%2Fdcf2hc9KDSixP8kY7q4yFJj1Mvs10QdXsyIX0vYrIP2PK2HXi7ZBRrUJWGBk1JwMvhTeYKpk5lAJeHEmIkqcZ4TRR0lympKlzjLCnvAPdWJjnlh8mFR4k2PN84IaOmS67qvXz8kcQDJGBrLO5caABqqyMGdoKtf1TG3htP2vrsMXesV8%2BrOHXt7IIwYAfTQN2ODYR&X-Amz-Signature=48d6cdcabebc96e43d345a78ba96eba572cd8f7bb3f5c84b219995efc0132abe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
