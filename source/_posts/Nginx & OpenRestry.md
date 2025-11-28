---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RIM23B7Q%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEx9lC4915yce5%2Fx5Abt2nRcSqFZji1QqYb0pky40zexAiAjiNGq6Rm2Je648d2OUgu%2F32AEczZKmY%2Flr9K642ScVSqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FJMWteYToHr8voCyKtwDtQDLVCZt8RyXZECfXF1louGHO8wBeeBJWpXZHIKSnZFV7e%2FPEzeTaGTXPViFcas98dUeKiuHNGRmtH50qE3b%2FD985rN8V93tDVSUAPDiVrgBamcnuMTkbjDdqTLvbJ2bl2HRUXjS6Ae7DJoFLH25n%2FH5tGOxEfen4ij1pfPe3CKEYb4fJYRJ5peL2EwcnKOj56PdV0eXRHIqrHLfNt35znRqS3BE3mEfa%2BdFgYBKsCLo3Jrr9W569BZFf7XpGKnWwzuEkpIPgl3NIGbKs0Iv7AI8eiewJbpFhMvp0yCnVUNb7RR%2FNdQxx5SeSLFYgotgyiH3aLBhYH9ILBlQv5OeJOWh4RPA4FaCMyTDrBz%2FZoQAt1YV9RqmzeFJQ67ATdxz4QhWRRSnabyU3N8RmQb%2B3pLoAHHJAwAeAljMj9SK44bU6IspH08XJ7LZ0i%2F9BJQyQN9dSG62s1C5b4n9Ouu9%2FbkDI%2B20Gm%2F7kmN0xUH1N%2FUac3GEWB9sAZXwiN1pxo1GR47RlHsAPU5I9NLZp5QQF514C75hpOeTYVUfmV2WIFtwC87BD9WKtiefNhHq0TM3VsfbQvXwgJhLEm3V7ACmm%2BGpWmMKSNGgAyan4dEtk5fAzaxz3N1%2FWmF1k9cwmt6jyQY6pgHVUV6Dvo%2BG8x0tnxoxLnATPNS2H8NSUeLn5jwde%2BkYGnpVa8NG%2FauehDj3aWIs28uxO10C05CtoNHoeZpd44eiwGtsBTBKx5uy3Wb%2BtS8OtX72%2BB60TLRtVWrng3x382UU8utUbcIMSk1UkVWdfleJLACljI82MnALIISaqYFpFrFWGEpYVro%2BU%2Fkrm0xr4E1tpsIUhSUsd9aM5WkxYMPHVQY0O4%2BX&X-Amz-Signature=226c9102cda87c0b828b5c2976474f5c94347d052bdc2b30b2a3c10cf381f881&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
