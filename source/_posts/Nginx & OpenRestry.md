---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX24DCEB%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T150056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIELWZC4ke95F4eMszaOpsLbZXUUBvrUNfmolEQZmns9WAiEA%2FuTnnnVPHTZ1BVphJyu9Zt%2B2W%2Fo3jhYsTERmxnckM9gq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGNLTQjZJyF1ah1u2SrcA79Pmnt%2FWr%2FWytEzuYb7t8%2Frq4b9grr6ZKOqNzTE%2BvXEex7Z5D8KHeISgyh5LLKR%2FU6Ow215gw3fk%2BaSVi70i5v80Qsl9USKu7kt6VYyH%2BA79%2BD3BUEokUi0XyuqRHz3lznZzdfKNzC3Crl0Uf7ufQWoQNkGXIJDyXaSzD8Pv%2BuzkQKNJyJLSwDJl2bIu0wK02EHlmo9QEKi4WgE1nR2SDUEMBJjO4sp1dSm%2FLYeKF2ADPQByRBsaeo%2BoA6Cv8Lv%2Fr9qTXbDJTqA3%2BnqR0PhrNfzZdXANavmS9KO954R41nnqICn31pI47%2FhkoEqws5kEygY7rxHWecVf8WyQv23AHzwTmBH8ckuoAwWYPtkR520xFIqs5pLoNDFLKLOpyTbq5cr1zA%2By5XwInhcNHl%2F%2BETOG2hwQ4qe1rX%2BdK53e%2Fmv2g8PqESVX6A09Z5H90lZJaI1oar%2F%2BmfYMyITzW4XZ5KJw0FTubvqa%2Fr3KuFsgbcIrrlesplhS2ewkexO%2FAxwYt3CJHyqYEnBVFO%2Fy8JFV0NS%2B6FP7FWz7nWp4VCnTShvaiD04OB%2F8YG7XvfXyGgN1gV6BsmGALqT9PiQW%2FGHrVQmfATZp39Agk4y7FcIos%2F6TY1Yx1GEQRLTYNySMPS4rscGOqUB8Rq%2BjFD4jqIlfkYjxZ7og2MeLnv3955WUyGVhsaQsYvEZVQjmj6xh9szFOx%2FEvBNLGhwf1mbBa1rkpMlWDE70LvtDxmKvhbGZyqAw8Bm7nIuy9tSb81D%2FB5YoPmwsC86%2Blt3yEJqVj22o7mEs1yNh4ME6NJEbViIPQWCZmobuzz1RUd1Y2oIAn3nQrWRnBr6REqkZiEkHFxEJhPSm%2BDjdqCbPgPo&X-Amz-Signature=4718eddf079803865ba28759e8709c9c76a2773dbb23f6e8f5c926876be905fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
