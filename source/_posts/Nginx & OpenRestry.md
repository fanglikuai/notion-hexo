---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HQCTRFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T130046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJGMEQCIBmTeE26sWxwMHlygtQ7Cpme1tTHnZfhlT6gm6vO3G4cAiAzHWZooD2%2BRFDsHDxM6KArqIPtXFSua9SBWLfJHGITOyr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM%2B1JYgXo7Sdir8z7rKtwDXmzFIbR61hciv0X5of9FDC2QynqvSw3oHBblFvfcvGk4AfZP177aeJcbLPikxMb54xxBH97ToOQbeQP3w%2Bpv00ra2SKBnICt1qoGWCzuhaUMe8tlcmEpX8LtdJYXHt0pni6tOwhZ6ZHkPW0jRkCLNTJm16z6gyYbwJB7H2MGv6MFsr7GFIULo3FPyp21hlayfob%2Bz%2FDQjQXOwz3gYY2CR04JRxtxUc8uTdENkfzSX%2BYDtC67RfZt9Aym0c%2FUE3rU5iLC2%2FS6SXc%2Fz86lWK14cRCPJJbJszE83arAhcmEv7KYzgKnndSfs30PBq8GxwxivkoJ5n0q7CarruGAhaWC51FK%2FFaog9twNx0S0MJZYpdLyJCwbQq3jC76%2BQc56k5JJFXmFIMiePsFZxN%2BMqWof9KN8nJxqFpLH33JiauG8Od8EFv4nL1B%2BqbZ2tgPIMWqm57IT5URKLDQUlJVfnBl8sztHGtnc0I0UV4zAsQQyET6stRsnjAJrkltiTy%2BMLD555tqXNGd2fq9vsNVxRcAmIaD96lW6KHQGFenh%2BugTOc1dyMhK2VAMaICpS380yA0BY5ZNsJVzajXy6VeWLo9e%2BHiCXrzLRmk1urCysqc82qNZHv9r0cAW0CwtCEwt%2FjdxwY6pgG3gy5gtkEytqhe4WdWrow%2FdjYgePIKr%2BbER%2FIdQePftZ%2Bqu9ytF9bp1eoVV%2F%2Fke27IFFAtbISW5cDRVv%2Fode8XShyc3xSiNQVBg%2F%2Bfz4AorxtadpBsuG%2Bmoztcq7zCeL19FpUewnjXvdtfJn%2BQU4AyB%2FaezFJFnbWuLemWLFMhEMuXs6TYheyf214AyX8m1F0h8MkWYAft%2B8Xrk1wxu7gBM9vOmc4E&X-Amz-Signature=df739e88763316e28e752e1e5d98917e46da673a07327d079a46ea06afc8cc6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
