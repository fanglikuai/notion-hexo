---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666244QSPF%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDI1Pw0HaS6qItQmr4uhXlhbDHBXAt9i0nqAulyAVuZUQIgK6zh0eZARwhmsg07TeVxW2hYfh6A%2B7RcqhDJx9WkTw4q%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAWFS4kMsrEh28b9gyrcAzGZa1y0UvQanfiVI6Ium02HMcSBDS%2Fhunn%2Bd8StGB34CyteJjIYkALzxvuNxCYPBfWOLjU%2B0rmL7obvBABdFnpk301WM09Dx%2B1D7I%2BsdqwcqjxTFJhHqcjW1E8EZv8Vo5snj7ivRgfv39Ixkhv6rQTX7DTtbgV217iVA49PZjxom%2Bn7%2BUGgcpMbVsVXWSC9gANcOmsUGGHKG8K0K0JAYGuZ78W7zLztABLoxsExz0HnM6P0u6H8XM3eZK6yd9eXI1wunjLN%2Fqrb5c4FKNksW16n5LRLBN6aXTM6IpT6Ft7uTk59od7FrWVrRcoZFKIX%2BMZezN03SyfOi5Oq%2BDmib9ArvJCTpyCjXj1Qa%2By4K3YqRDFNBUIFotHBDH0szA85R5g4Fwxnm7sM8s1GjdnZtDk%2B7RQvghvX%2Fe9wh6%2F4Aoq2HNQxFJm4AIHcZ%2F6FV9DHHXF3EGTzZflOsb1a2FFPp132GeZ8uo1HXk1bVM%2FOIZ1MUO%2B81kcI37OWPQ2tlgWoRoDNx%2FsnTEgmMFCW3hFA6Ksz9wKUb5RkuPda2kd1JQixn1%2Bql3ll5Mr3eKWTC1LHLpQ48F0OrQSdKaJD8LG0wRuxPS6pSon3vkBLuL8uOd5rlkDnMx7v3aRErs4bMPKjiMkGOqUBURy3o%2FojaFOGGyVvmRONMCWgSzGxLblyG03CiW0cpeNDaIRJYsk%2BbqewicTYpP%2BWjxM%2Ftuu4aqxkCLmzYh%2BFkTFy9aLv7PeXvHDn9QnNLAYUqNNjotPtqtoNx08KvY5x8EQG6zIt3CpbzRcNvzStlOxtgYSk5ecgQFSez2LAPF6HQ7KqDVNJjQC8m8yzashAFrLkcdFGpldSEmcdqYhp3r%2BoHarr&X-Amz-Signature=0695478d6f90611af6bb823ca56fefbad79af1f442df7c04e276fa82d4bc9232&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
