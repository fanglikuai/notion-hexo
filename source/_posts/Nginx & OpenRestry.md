---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7F3O6ZN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIEuBGhWa2i61YpMYmHTQLv6PGnUSH6BLXZYPNGOsWfudAiEA38XgOPkd8N36Oe8Bmkzyl38a0P3oXJmAlykmjHCtqqMqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCl86LnEFxYGE2bj0CrcA%2Fp6BysZ3L%2FDMOqB7jJ%2F8U2pcrvA2UeYOlgeftiKtAUmCMgwiW4bIsrU%2BYOMZ9QFI3skhqE%2BIK0ZXQmJu5l%2Fgx%2Bhib4Nlp%2FiQnH9D1DRa0bLJ2SFOt0MjhxyTeWc6v6%2BXWiNpxOAWRVoGMF6VZj0N%2Bh8XVMPzIx%2FzN55GeZnBbMp3gPzYBQDFR7CT7Oa%2FQ745Q1Z%2BWUaz3WON4vKHtMOPElD9LZO93uj9MQpsn%2FUOHbOTgsyoudyXyLWt53I8byYOf%2Frpphudnb6YSjtom9V7WVoi22AVeVCvO172mRRYBosZQwNGoSWYOuu2%2BpA390DrCFgkI0rliKWe32fhTjzyUQALL%2FpbgMF6nDG%2FL1sweCNMNlFqFiEvxzVKsUC2eX5Xcwj2P47Qafi3a7IpzyrnR%2FZmIM75hklIvQykOOgsx3%2FSywojYEUxarVqdV215ImIDaRDb37lBflYUeUV6nutQ8oPPOlxeb%2BaVWP%2FRo5cAwkaOVgSyHf9H8a3dqR3%2FgkovQsTD4t8FFI8u%2Fc99BLQk%2FPrEHko30fXEKk2i2sw8Rkt70KUqSQ%2BJvvLiS503TUIlLvpWxzbH%2FZxMq3xTKwS%2BsIuAZxYjVpxZqs6mdy1osqzdHL3a2A0EVPzb%2BYMLn11ccGOqUBOzQfyqrTlAbI9Au6uZpf18neq6xBDIVG8JMZOwNMY1HJsbgroozMbsGGEOMOliFvE8MG9EzEpY0o%2Fz62HSY2NDUlos%2F%2F7jpq2%2F5lVNVbnJ7V7nO7e%2BfjQNZ5vUdhWqprQtvPah0CUUu0lfJYbQSslG%2FsLHYPmhwMTkKQxm1Omk1G2Do5IDumeTlNh509sDPQbrG76FOz9TlZqshiseTzXTvPHN9Y&X-Amz-Signature=f0030f8fab8b37f0eea7a44aeccaa011743dc6253636069b09dd9c6094212453&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
