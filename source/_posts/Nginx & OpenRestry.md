---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBOKEQ5B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQDJYsfgoL1sPnRa2Ks7J90TqWSYOm0cV8RXg4vgJYJKbwIhAOgw5h0cNhUDSgZUHZlH3%2FUjneWGehu7GyJbhd656iMCKv8DCEAQABoMNjM3NDIzMTgzODA1IgxDBrMGS%2FLGdBnrQwYq3ANaYfhTh0hlEbOCZw8e0mq07RBjwaOLM9YjWI6GBC50YAjuqeFTcQK2rKDLqfShxuaZDkrYrX3SXH7uLSkkN52Acz1fRxxX%2BFzexzB2DBmEMCWdiFxeB5YzyxjpEchFX%2B3fVq9Ne2okgFV1P3DC4QsDmI7vu4%2Bk0o4f1CzkYqCeQhJcxA5omY6aGB%2FwPihQh%2FLstLVAtZ79%2F74uNx0ITcMTs3lRMrbvogsiFw5tlgtC03%2FU7cMieVuhHhes6iSGjUJ%2BEU6lpqtamMO3jnnAMdY3UqCsnlKfILxLduLXxro4jwQ%2B8Pb3bG%2B%2B8Sfu5dFDoen0I5d9l8WVyGGUxgWR9g6Nx0nH%2BGkUA%2BgH37l3nI42NPjvm2dlQOWTksbt6%2B8GPnBDO7vy3fGzMsNAHdBRGDLrgVUSZBSAonFj8GPZxl2Xt9%2FdXJzIHm8kZ91hNccT%2BpndyUfclkMdEwjGJAHrxH6JEn4LB8ksVQAqRDqqG9wi1M3P5ablyMSdx7WL9e73HakqDrql3Icva5Xnp%2Bve%2BjpcuNb1P%2FiFdyJQiKBdpDUcou3C4%2BhcV9ehqhT7yar8bsNjp4%2FIPh9MF%2BG%2F4croD1f7G5kKP8m0dEPvxTYLrO7fl3vdPowLT9u5ijnhBDCOnNTIBjqkASfGhgX4CSpf%2BPZX7RiH1%2FTCPqkMiVydA2%2FXGMAWtME3XEINs63nGC7bYJJWjeBwvbKrTx9ezfgIgpPFwtTKKpqc8GwLOGs0RkOyMAvPv%2FRm%2BWZydIPdkeav6jZ2b8Oj6NX6hKeUO%2Bj%2BnWDWXPyLmz1gyXK%2FEE%2BlBtrimPBaFvV8T%2BHMC8qFf7YP1tSY6PZcVZaawzrrZzxqVS28c3AB534Ndc48&X-Amz-Signature=44fae0e9a8aef2440fa66916290515d8a7bb605b5c224fa544980638cf53aca2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
