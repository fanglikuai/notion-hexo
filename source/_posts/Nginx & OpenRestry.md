---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KBIJHCN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T130038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsN2%2BBbxvsNyw1WJ7fec2E%2FYBpAr06Ozi0mroUz0DOjAIhAP7Cj%2B0WsK0glttbyoyffetJKbEqfdIZAerBYUNIHXzYKv8DCF0QABoMNjM3NDIzMTgzODA1IgwyxCuEUjEtt4cTplgq3APm9tr4sGSLz2Nand9PSg%2FxUCr9okr8T1mHXgMd2Fc63xDtpKaXpyaIZoitugwHyGOvdWiApduYwfVpYU9ZEP2TnWzsR90WFPaZRxVGNngmrntRni5rhWEKP0%2Bh%2FI5vlcn88fTnanrEuCK44EQLMupdOPRJVPlgiGQ%2Bv60GZSkv%2FfbI3CLWrTmZNS%2BsilOXZ2ZPP7qnZ3fHNo1Ohpl4g%2F%2FkxpmlW%2BPgDwggrclOibuQfnwvGx5xmiay9lm1e3GaYDTW%2BgiuvIDTisAvcjEBv0bOM3bneAwq8bbqPit1nB%2F5%2BAAEiCXt%2F%2F6aLNaR7ys2yd2lGVWQ0fLWgtKKI1SV7YlnDsAi1RCN%2BiCy7QU43kAuBDhZ0yMTUmyDaU8cJqUJLeILFkxLwY9XASwgQk42SnYVc31CmegBMyeM%2BbCztyzJtjbeAmbyKkZoHC0sVQitUodoLOEUqWcLn%2FPb2n2sjjbBBQ8etvTfoQ8H3oLZCDH9VRxlJ9cc53j6yE4YLrETaBdQV78x4EzD8Qa115f3z%2BpP9FaOeZMqzw4tZ2EfuqfcchRhRIaGfqBgHQkG8wMvvwIs76ajXxqZC6Jmlmwot%2BryojkvX%2FzTfPcCsF7dxrUQm97lh0yopMxOGqpIgzDm87jHBjqkAQVqFFkut60GBlFCnE5ORGe%2FT8a12mc8vpcg%2Bqi6qiuT66IMu8xeUE6qvnb%2Bbz6AcS1VNHEAFbkYpmgSZDUHLAwBEY1%2BV6A9bombFpmiN3ZVLEAGXM176l98VFEgWBpUg1APH04qoSayDI1mbhW6jNZy%2Fk8UgVTVfz0UuEhmWQwYPUSuptKBN6r6xOBRisWkgSD7aVtPDBBYpKSmHYkP7BXnvryH&X-Amz-Signature=7f1fcd26a9e8fdae88d6507f155b657dd520fb63622ea2d8d532529e5af077c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
