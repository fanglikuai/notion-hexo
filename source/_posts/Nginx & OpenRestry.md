---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XHV6EAGU%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB3zxrdou64z7Oo%2FfBR0AXnoQm6qM3pnCVGAM%2BevKBIlAiA0vS6bp8R3BsSlgNBOPvVuBefw2qD5LFk0jRMidzjckyr%2FAwhuEAAaDDYzNzQyMzE4MzgwNSIMaG%2Bog8d6JlNT%2FPyDKtwD9OIFpBZxDmvO7A6JLPRfjteTUpncrwmFZy0xhmfTBQYc3dghOehBfvon3iFtTkbiGIrDhcrbo2R02KvmVMcMFU4PsCstpK%2FvfHb%2B5hNJ0%2FQkydYTTf5ny8p3UPU4NgeOdk94sTdBMPt9H5tKnhE4id9eNFUMe8bHIG15K7wLP3a%2B8T9ZzBK5FMEXTJOoPCzQNjjnqdmueFrCGh2pkToj9rkzKN%2F%2BYDVBs2D2edjgug%2F%2BlDHGFuyg%2BSe02CKevf5lBfY6VNjEIRSwFV6NnxzFLlCB%2Fh8cXOtJXDkTvjUL3WaUZKQrVjlgGqdc%2F1iOMCsrWJtvbRI4vtoKJF32NzlODE5BTDp2CjPW75GNyuItPmFBJeSJs21uoC7OAJLQIuEve6Vf8uvwPMlpKs9nlEautKw4JSisD1b3ujfWdoXQcHEXfLyL7qLWXT7fh4nxBOph%2BKPyblWep5wt4Ipt1q2o4smJTvh3mZEyVgwr9%2FgRVeW7CFk7xSLtVIaivPCmbMH2kVRHcCeZYLddw1Gh%2FM9czghgWGSxTiOreDA8FAPqJptSooYWFGtZrVoNRvCwUx2TZAngxg4GAdrKmg4lLzbxsweG29FDOqo4m3IO3yCq67oG1SNKPz%2Fvyi5ozZsw%2FobTxgY6pgFQo8Gaq8vqHI4WmAZdu0kgSM%2FmxMVpX24vtQDtPaZ%2Bwfd6bU5zCID%2F36yySf%2BOiw8W0snlXoFL%2BQMJOej40QgS%2BTfEyL5xpbqTS2vNhPyxYi1PIZQZDlm6%2ByyUuuxJRrZ8L1nnok0ILm2o2L5pQ%2FTlgsTnrorVPxivOzHFXZTa%2B0hwrBjK49WT%2FipQeGyo41WCEmfwzVTTuES9xcNlUfnscs%2BQidqP&X-Amz-Signature=df7169a6bbf400f9cd2b89e3d520a829f9567c19c8477929484df6a05a7515df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
