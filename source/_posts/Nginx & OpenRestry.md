---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VW4X5VFF%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJIMEYCIQCom59Uz%2Bn9r3IUKr0xFiM2d7H%2F%2Fk025zH8TxH%2FreTRMwIhAMuV%2BD%2Bc%2BNUiJgDjp1sRojmpiPj4LONXSsSvK5SR%2F7PnKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2JKx43gZJ%2FbjcRZUq3AOl7w1z8G80CrJM7S3JNxKxt8LknP8l2wy%2FWNLXZCZZtg%2FJi4qI3BFQAy%2FwJMUjbE0end1VH%2BlxHgkSPGxBDSd%2BrW%2BeHyWwhNoHG2FMurU5UJflKRcYCyguXDL5hFmh%2Fw%2FisTWOVaIb%2Fje%2BcMc%2Fd%2FAYZAjfeaR5LyztFn2ibVf6uQHmn%2F1tki%2BR85xsESySvR0CU32v2TObHpFvQSmWPBP%2FtPC%2FJRE86zFQsJ1vfIer2U%2BnfQD2VMdqtYZT%2Bwre7yROznlnUuA3GPEvG1TWA9M0c8u8shUvNH%2FPs5mGgPJf4WoqCOFzCTFgkgCf5axmjbfxhd8VlHq74NKUw%2BwrujupBWYpRqMcepzkn21VLPJBAyFiInLsEvhm%2FPcDnGHpqjunC1NfqbDwpGpbAi8yTKP8icl3ECsuNkp6ILQK1onJXT1rSioCYkngYHBgz2p4Bbt%2BWOqBbkNwgPP%2FOr9E27IS7a8WF9fJ9lRmb1fyNwIyUcX%2F04zqbhgtigS%2FqXEckYgHj%2FGhpoDrYBlKiyFwIgrYFo28Gr1YQpbs8ED1jHGaN214rhUz2nkrady0DQR3jTiMrXXSlUHLSPx9QMBmX4eGKWqLwtCsQ7egl1%2F6Vlvkk6xyCxp2vYhh7YyofDD54ffIBjqkAab0Exa%2F1xkWepxTf2qWg91l6NisQhwf4mRl2gwhQoqzuxA%2FY9l3pyzkswPc0MDHA3tNN8saBPYDLoLuRHL3nfuebFyI82EdkljLyeMaPJcjKrYnMRaL8HZ4dmqESoY06mxn4qlx4XCwHH%2FrS6YjqtYiInITCogmZ4mbTRykq%2F%2Behz1LpNtP8BfDPeTXQDJphTGh2GWGNyAPvnusZXmcP2I9cMj3&X-Amz-Signature=fbf537cb48f81364e2b6b03298497d4bed6c66b6a895bc9cbe91d5786a9ec477&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
