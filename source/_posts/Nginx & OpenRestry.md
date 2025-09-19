---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBX6BL7M%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T210049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJGMEQCICV8doqNAUa5%2B02tWRIyCiHQsmkSdlq3s8e%2Bm1fLEosyAiBefzQ60Ug%2FCRCnR8JYPiSe5QHdEeEHbQQbOUFD4iCeHCqIBAje%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM9xYrXzn0E%2Fs1%2FRS%2FKtwDyRr2t3H%2FTjzPkrM47j4OJU3BDEetn8sPfGxuO0Q6St%2FyOIniO%2FhnQMcvnLwc5sjDf9Ix8bSkbOyVdStL5psGrOhhHiowq0o5XnTCoU%2FzGPzR0TPX7NS4RFW%2FcdLIYMKrsyMyJzX%2BBsvVB9MdpesdRovpt%2F8Ee6YLdDstBncOHqr3rFCpi2OvsBJoTvk0ewuksc4fFITl1KGJDh%2FVOgiAmiC4fUBnxg4Ds9YqJjvrpkUtIIWeQ2azqpff6ei94Djwe7MOSgd0opPt%2Fd3TpMpimXUXBWwxT4UEiE29c3nsbhsOUDatDaj9nQBQCT3av0JguxxnOkR2OCuxEdL40cnrSJAYIgah0xwWESl0m8sjG0Kh3jWM5tq0sDXyBjgfYzoHuztucQcVKfm1JKu%2BvdF3I%2BZCAp%2FIduSM3v3UuQaHJ6IsGFgS%2BZ2Bc%2FUkkJgYb5di%2F95TpIo%2BDlIG8iDeBqxbl4Xg%2FrJaPLsRFlxjxYxWcLglsAyMP17GAgGWVYhGu%2B88xrzy%2FA4xGhWs%2FGzIImzkEvl9sFU5OSmWbiSikhpRLjp%2Faat3yQc1T%2BL1egLEsXAQ2BwJW4RlhVYBAPe423sqobkZ3wQUhPSenUY5zi5CgKAgeYRFdOzMPO3k36ow9Ye3xgY6pgE893HzMLOzdUzVtlq4CZ4urEeyHEM97naEL%2FCVnOZbWqjvnthdlsz6PRcmsEWVXjKO6HzzaU%2FJNmwYmSQsShi0L2jPhQguU1n1LsB73rCvEGkoSLwDwEHtbtjEcBxrJ5HpQRoJ0IEHiMMZmhET5Ni6IorQqG1tqIVBeXrBeSkrYZXyoVZvhfqYg6QaC2cGvrteOQXiABkgcykBuvJBP%2BpCGTw23Isa&X-Amz-Signature=7101dc86c6d53a220c32ffe6a9b602a1e93814c212bdce47755e59f2386f6643&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
