---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666XRWQB2J%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDIismzawN0PCxGIYuMiYKj4R2jNsnlnQub5y3Vyu7i3QIhAOeNXhWJLNbf7XAW2WKO68rCP5Uf%2BXRlSQdgMK5yOOCAKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyYDxobUlp0rgxjGTgq3ANcpXrRnZ6ovNthTiS%2FJ%2BSqDvwvAi3nnEPLfSr3PxN2Rp7hjXtdJYeoa%2BByhJhOJaIrPMmfbUueZ6UA%2BDU62TT%2BFTzi4M8yKc3Mh6i4p1y%2FLyrJpchluNdN0gGU67VJ3WGvfgn208sLOeHaCJGwYsdhTjr%2FXIKuCEkjrMo%2FqKVr5MgYLw3VDhy6foxNcw9Pxb5akqL9EAKdWAAnPaZyZF4eufk2E30ETdxp4LImJHfk4WRaapnZjUB5lTPkOq8rAiVZecoRPHv8ZICAvM131nMyTa0QZHWxuItuwJPum%2FKWNWdVYhYK2i2ZHT1VAieaFJ2mBBzLu%2FrD6O%2FMTN%2FZwP8USJgCnF4ArrEyoI%2F0uOZJGdDMMefEg22ypVa2p6%2Bn2SyhNSPGp0pEyisiOH6b%2BUK6%2BavEgNt%2FWlyelRhSe3F6igq0nrUX40rIo08r%2FHgOAPYlnjPVE%2F3yF2GxRZZTCsnrxmRnm%2Bb7pKzn172EA7sAduUY%2FP%2Fo%2BdkumeCfYzZq06t7vi4lz%2FhJ2%2FQbbtq15UrzYJ1ZIJmvEh20vdBaF1973%2BSxFdUSVT8ZD7OgMIS1IUO5HScWZ7g%2BYt1lUKQTKk17mli3q2hE%2Bk7ZACck%2FXP%2F94msKpOeAMx856F%2F5zD2h%2F3IBjqkAWdzP29RX1MBdGfkef8CZod3fRWdKXBw2lPLi89gJIuKevC7KtbzJQ6jSHBomjGsaoPS4C%2BKeCvRiwo0jr9lhYB3MYVZ4W2zfLrdxNTMx7FvPIzgkMECgL6Cxj1p9NpLwp%2BDQlDM%2FLiqtCKGJ5SNL2bhBaKikbqVIlQaGO9Psaq9438j%2BPP40mux3%2FQH1lPCL7dbmOjq7xuUS36reDs8IIDSSSP%2F&X-Amz-Signature=2bc05dbd19085f18dc84ff34861ef2bc05226872bcb2aa442b3d26c53cef9528&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
