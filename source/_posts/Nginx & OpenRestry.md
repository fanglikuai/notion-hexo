---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666EZB64GZ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T130040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjR0VIzv1QZ3Yf37Gxj%2F2uLpKe3bv%2BBbDIm%2FvMpDHSpgIgYTcezxOPiacycFiUCW5kVIv95WWLg9bKDkBTELGTmwcq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDMIqJhaw%2Bam3LlUPdyrcAyfyF%2BtYfDRzRQtpPPF3Zof0ppghHEDyP%2BzROqrGdBQhbyfgMxKhOFSGCahIny5JpU5qqIJ7QKlSLuBjWmR16cQOuz49nzDkUXxIww9b0QggeimcEEpVCtnaEzO11VBv6QyYm%2B4dkZ39Ep7%2BY2nBJYrKgwCy1IPkVyCFIRZJyuvvBHciNyVuyMMTJQu18WhTqef2ug6XrnI3uQADm1kkoVozq%2Fxx2a205%2Ff2ohpNoKyaXV47mp4fLwerLJodBw%2BuVIUqqPeBnuhUzRyxjZxnOJUEUUMjj%2FtC8xnwsf%2FOpsYrZRbJ1H1%2FAyzjeP6QSSn9hsAOapLwRgArWYwoJ7Pk33V19qX0FfiaWtILjZYQ6xbqyOpnAaZNC%2BLXgXHGfl%2B9xWQYiWZqJQn8VP9cyZ7y%2F%2BeVtSJWXhQJA4EHxuSvloHBUWKk0snGDrMjZzzgF95QUzlpjfv0%2FSPUa9aspJbWfdN0Vmn8KlXRq7xjUYZ1M0OCEizz2cY71TynXMSvaYzMCv9fum%2B%2BhwGraV5mwlPRjFJHojAreQEicc80sN4RPmT87otB%2Fku5ac0bRt8LbzORiA5qljYFKo4d2X0iEus%2BiPr7U%2FvAquayECoQwnC9ZFh2n7dL7wAa%2Ffpv1YDeMMiiv8YGOqUBu1UouOHNYDXFy2HJA2NzTvvPJglhgqx6OAqR6q17IjMf5wmd8Y1GouE1eN0fcL48GBJkp1NDwNkbX21jmVbEKak4f3JiP2lwYVyowJcozHcxkQdZdPSrL5HRCFOBrwplWebFMRjVf7mXO4lgjIQllAaw7fRvf3g0kiqQE6QYoIdp%2Bjav6Obp46gZMtWhqaRcgRLLuWYihIc2pGJXN2t25UOI2dSo&X-Amz-Signature=5407c4b5c962bb5e69db091898df1dfa60ed8f701841ea30697b406257f27dad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
