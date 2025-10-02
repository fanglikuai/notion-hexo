---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHFZTHNP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHDJWIP%2FHFoQbEOMk2pCCA%2F9MLG%2BG1aTe5bs13Lj9NNdAiBzkO7LRgkFBhxCaEe4sUHYyz27idt2EyYz5MvPqSz27ir%2FAwgyEAAaDDYzNzQyMzE4MzgwNSIM02oqGKtdhhhCSRjtKtwDR1n5faGop1k%2BjqnuIbhPbzGCHRvjtQ98m2yP6Rfelr3LYG%2BWi5BbVILhWj0CLRfC1pVICoFeThMBtjCN3aM3dA1%2FeJfp%2FtGfpIZz6BDLvnuJQsD2OOnE6ulPOGgFvN6fTTOXZc1cfzCRg3Fd8Tp2tSTqOEDln3v8K2OjwlxR0dQfY%2FOLwgIJneusuuAgleTafmWu1AIDylPuAr36HSgQvh0SJT8tekoFOrTRrXYWVbdDmt2V%2BXDwh6eOm5Fa%2Bw%2FlBR2thPzMZU4352inldlPNOTBzHqOuuz4dOdJexwAMbJC%2B73m9ClLEN8zXyVP%2B%2BNciAAxHb8xgOnw7pc4wiMoItQQuwEwBNTEwhpUrk4jlOopAQdUnz7rOf9HVB5XccgEcS8HHtf0w6SDxvIICCzzL0qrvzW2Rz6aUBZ4Yi59pBzHX2Tgv4Mdvj0bIHVhdgEtXFXosrI%2BhylrLNAuSs%2FkJB%2B5NddzFm%2BnE1iGB1ZDc5j50MlzlL%2FQJjzA7bx%2BdQ82GExRXPjzh5k4gHvUB9ieQs9bMnQaNYDTgieouS7BfCaFKKzNqylvW69VXJpRe%2Bu2iWt40gwT0ZMR474cCWjwSApahTbrGRuorkwLv%2BpCFW78nFC7M3sDCwAjs5gwxc76xgY6pgGVlCoQm6F0a%2FZleU2WsvTViOyEo6541nl4zqyckZe4%2BrdasTs07UnhFiPChy4hfNFVyeSNu1UgbUzSCV%2FAYiq%2FIHwLa6gEeA7sfSw2dcHt%2BG9FmO00ovFDi%2BmUMIVLGmrq2DzX0oxVoyxulbTGYyDZjeTxmA7eAUw367qa5TS6iLtO6sgDplbEnBddjQuJZyvi%2Fvjc6JQvRXUg5AFnJuEqUElpF42S&X-Amz-Signature=8728d4f40e2abec5f85c6cf9582fa1577256a9883e8c057c2c915202cc243edb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
