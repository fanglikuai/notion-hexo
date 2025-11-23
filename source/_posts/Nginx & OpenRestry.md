---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WET32FYC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T130107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBk00ZUExktKcS84gkQFf2mkrDZm%2F%2FKS2L11pMH8G0JxAiBnwPWZR2hcmKoOd2BEBleACUCGk3Sl5vhytJ9WzEcTSCr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMhx7BPb9jS39l16HYKtwD8k9tzyAjkacxk1OpwYbmhWf8t0n23hPvfPL27rRX953dG8oU8GhNgWleKkFGQBpEtM3LxFM7NdNCon7Xfy90OX1DdqpqOC3Axx9WYHq%2FnbSc5dxYEE34f0OFSZrD5bpAXP81LDmlpORoS6X4aSdm6btPe7GH2tlAkPuDDWEqDCkiR3qbrJ47JShnnce5lxnV7979cKiZq%2Fnp2tyqX9eAQLMPXWOIVXg5qeFETrswKFjrkdkg0rWjkm01oDTyFjgYkAoNJ2UND0qcLzxF%2FLsk%2BiueCLO6jIWe3BO%2FVisinQFtBA2zhPdvPQkXADB9ND8kF5PDVPhRiEAXobXu13XbRoBDn6HJF3LBcbDUlbcksL5eGW58PbySdcvqd%2BTQksYYURFu85Yu8GgskKBrXBnTW%2FK3BVdG56shR9dmCSJzsVjfyVG2bPvHtf25tG4d8catbnFXTwH9JpNKVTi11X3FrY8ASzl85TAAbD4QR8wbFqQig36rEwSSZPvsDKFMb6Gh16uKjPq7fnVpdVzVV0hcjevIBMb17FFWzENzcBsVp9X5p3XLCsCV0tW3fSDJSkkOzD81msTPEQUaC3F%2BMMO2VwVUN2zd0DHaDyuYqVm1jwNZrXv2dNtdSg3euNIwuJaLyQY6pgFFkOLAhK2ZkfEY%2BfPH3aYXTibUeK8VaTqhiOrfgG%2B1%2BNfcFmCDYoplUNSu6HxG7xXpBx6A4pLF7F%2BzY%2BIxRwBjNUHt%2FpWG4rTM4qKHkgkRwXQvomabkX4WR9M%2FUy%2FprLz%2FlIms83XeI43dF%2Fv9vOD4RntScXw5L34Dg1jxVB1%2B0m2RVogIwl6LxHFLWzvcznZv8DEpToV0YEc8I%2FAI%2FLP7s58nk17R&X-Amz-Signature=a5052cbc99edb6fe24aac79317f39406061df0552b9e7d43afb9244af2757a2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
