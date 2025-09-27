---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRYLQQDZ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQC4kdRGJTO0rWPP4O2KtukQRphmFMgEo0zBjCkJWOC9zQIgNjFwzbt7u5vZKNZZVVmUjTEf9dtf6YSSxZm%2FSy%2BfAaEqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLc5cN2iVUAtddmqUCrcA09bSyFqf0f6jxOuOq%2FDUp1vxlxAvGuJDP9Iur757eohKQafH6tYd9kp0vGxyiCcPuRuihixzVk2c7GBM9wrASzNqyZaDpYRyEl1j3DxfiUc%2FJ93THnt2YSdcnicZJC2gCVqySQPbHswmvVGXlMR7zM5b5Xl874MkEfRPbyJOmf07BcmuhYO3ZdbZ5E98cbL%2BE%2FDPgeepmxoS%2Flz6e1PZhKyXtXcXpEqIi02wKUN9yslfBwaS8NIpZwbztEdS7So1VvTBpETUgfdiUwhr%2FlCGN2rx7MDdjXdK7MScMjNyJkraP5%2BdBkx%2FHaeIrQfni0RFwZHQIua%2FH4kln61VbXAKRvjH2pGfXaFZuT7tLWxx4EdiSGS8H33oHlEDug28aqPghFqwtkbaK3ZEv2HAU7bfDhuUjkkPIZPupurx%2Bui6MLILXRuF%2FwaFXlG4SmMlDwOa9tYNYEQfbIFnEVwv6WQFF7zJcYfv3%2BNg6eZWX%2Fw5fBYLulF2FZhfC1GH5%2BoulqrpnFn31NQS%2FlNaQJ1YEUb5x47S3cdxIynYHg3MQ3HtqFdPDTWaAiiQtWbxok%2FZsfrM1fUkZYTrKtMA7AMmzlMViBxei2CaY%2F%2FlJ8hungKGpM%2BcVOjLe2aLQxRcIScMLfi3sYGOqUBGHLWMgC9b6McP%2BH2Iep6R%2Fa5g2GF0zyCArEU7uIOBeq%2B2mMjcAUT%2Fo3ZK83B6GVBngsmo7EfcJe3iYczDuBchZ7LaUzI1Oe1kQujXfWHjU%2BCenUc9v44MEHfnRS1jfJ24ICqAFNvKjBo3tOE8RJJdPaKcn214xhoMt4RxqfyRu6VajhVEVkjvYoL8shBG70OH5SoCkv7IbpSrNi4hhrN%2FTCZmYF6&X-Amz-Signature=79aea0955070cf3b7e289e6e793d3c6b06aea7cb60dd71d41a28f5518c676b69&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
