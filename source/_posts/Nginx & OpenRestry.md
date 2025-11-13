---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXRXY5Y%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB8kAI%2BtwrPXCAhCxXgzg0lkGHY4tO2NUtfxX571EvdqAiBaP%2Fi5z0EevPDW%2FjsSKGmSxhSHOgiQ5lwRhZEA4Wtzpyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMe6WBOYwRdBHYxp7yKtwDW1yLBg%2BNo4AD3oOVy8WH2BVlrvw08FKgIsS64VMxuWZJtXnCSAuLCBnMiZ03F5UNHijn81w51OZJyMosI5re0p3jNK5ayqCrUGjmFP7%2FqXnmfqHhWmCTGsHBOxnPg%2BwFCDspb8TxkHuFzI2bwGOHpPCWd5wwgBCp4TTAmEVFZ3TxIUFRZxFgV5FnDZiJgq%2B%2BbQUC0zz51iBLz%2BQ2WBVcBwHjtIf63v5POROzWP5mVuyJuBdaH5E4JeLd97xMErk1Slz0XVt08tfpDK9szKcVOMm86ysrRBfD0ly84EC7PHhbY3VbHkk%2Bwqr%2FCg16a1DIb1jLVZZ2g%2FpLIyAryl5tn3nt%2FknEpfvOndG9tiADRS8i%2Fy72dZtSzoHJYiozH77uXoSc3EWrtmFpqq6OR70kcVEti26QFweEcbeGWfQlGCpgKR0xkYlzURon3YTr%2Bi1EcUvMr5UdppWEcbw3oHYnLfw8oT%2BeTsEypjI3358ySbKgEDVXbnXXC%2Bt6j9j243MMcJ2vLSgjCJeOzQHE3hE41o%2FdM%2FiPez3dODXl8r6Pfcm1sLYK8TKwLnyTLmMZRXSHcizVR1NRdaMQQai38UXM9LyLrTVGzsL994eM944JXsV5%2FSFnMjkRrIuUiO4wzqTWyAY6pgFuUn%2FPlfnXNqW7tCDWSYOrd4gSPwvIt%2BWmk7nRwrmAw7yPx5H5EHuLMAWGsq8BPv5fndhNAZeFNw9mBEoZXq6rcAVAJQv%2FUpRkkP926mtUuA4lL13dFBWAYjHRP8ixaWR0OiwREFkTHlWyPXEK4lL54UxlW%2F7Z9TWWtnCVwF%2FJeimN5siELhsE8ysOD598sa25UKzlBfxapYmE21gRO3ovd5Oox10N&X-Amz-Signature=b84195871442d3bb93a212c536d25a13533711b639039a294d5a7f73a9cdeec6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
