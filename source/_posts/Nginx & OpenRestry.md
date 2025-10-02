---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTBO3U5B%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWyJh7GcywTWMocmxSuH%2BXv3GJQHhDVDSrtY2S%2BABfWAiBOT%2F0iwBCiCgadlcUhwtbKq63ruNT96aiiPDC7FnCmgir%2FAwgvEAAaDDYzNzQyMzE4MzgwNSIM53%2FKh9O4ZHR%2BD363KtwDTASjY%2Bj5Gqz1K02kknj%2FP3I3h%2BYYqLWle0Dq2Yu6f4TDx1NKBySF4RRe7H7X8%2FNX%2BAg99F11XutzQbZ1uhr76AKnn7b1bI6D1nmSs4IO18nZHAmXojc4aDVylP36RQC8UsCHHhuVGXxqxPtbZk1FNuasZ3JjiquNkdVVjlLA4cwtbfhh9prkxkoBmm55YnXLCMCvmDJGAbJsYEZVHBSKGRgueOSvBp1IvNrnk0BVEUFobwJYbxHxGSpsMqwgApkriHxr3TRDd3qhBsTooL%2FIYlM3XEHjzG8hbSpGuD8VrC1cN%2FEXStg07hQh6fWgOpNJX3SRocaKd2C2zghhsK23uw8vsOPrZWbTOrdD0fKTvKBOokHQ%2BG6OjBBxGY1%2B1sbiHGyfgKCc8oVdoph3VRLKfz%2BIhhgRY50rf3Puj%2FFUQxnXYoDVU84W%2BxBAsGGTyqNlgnS59mABv%2FWyVXxmKm0%2BQyPmho3YQ9XblbdZwtcAOzC6H4JeKEpRHWB50DwIiiwsDUCv3DmIOgQMXFg%2BjtxFLKm0NfawRFmzeNiZTpj4UAvtH4jJWexDCdDarKB8BTQVB8TQBQJdFNYuqTSfqjc2fQ4BvxW8c5f6EPZ%2BDORKDDRprlReHVeKqgGvxNYw2ov6xgY6pgFAuitGRye%2B1RjNkUJP%2Fabu2Iop%2F2NgeOSy2MbhdS%2FTNWfG3z2ZuA%2FyZ4SWcGflxx%2FMC112fcqmfTj8omROtaOyuPP0nO2T48%2BoTfgOgxL%2F3eLHBceaWGyBXUPfli1VqFHI9WvUWMO0uqVQJ3laGeqjT4porwncwoLlC%2Ff%2BKJqpDmCGcDP56PwRsnGxId5Kop90kKauhz%2BG32cEZGQcmiaXpM4NUAg1&X-Amz-Signature=bfd05107aa91c7c988a9d7d06c8125d4573a44fc431be10f5e573db3bb3f812e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
