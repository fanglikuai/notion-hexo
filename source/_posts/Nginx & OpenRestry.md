---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624DJ54R3%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T190200Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEPOFg3eJf%2F%2FRoSjGmKzd1vIzOoJ8qh67P0OoRNV8%2B1%2BAiAGVcafvtPOR8EmIM5P6SWczSlzwkxQz9MSgqCZdgj%2FvSqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMudBv5rTKxTidZ9GKtwDh3XgNCTseuWWSDzkEJvN9RNzO7cMBi6dDM82tsR9Y48u%2FJw8qWC7lB88pxG7f7L4xBFcCx%2F5Jo4xDC8Z6eiVjKZJv3Yclb%2BI1%2BPhFUUB5IKYzIjODUl7wt9wnp2nM%2FJbIV9ad8AAbxtHD5fu2kKMMRgyE4JMdVm59P4ubJGOC98ntty8JWN7MmCOmdfBfS3CruP%2Ba%2FW3%2BzBNDaC8ysRkEgQ%2FatT5S3QRlqrU2Se5aEhacX9zN3Ixxj2RGboKqeaVSgW9D1E655QPTa6MgyCYCgW7OaudPULadfDeA81Zt1dkU7FHQ9l%2FlJeMH9sA4DrujvO3xmfrN%2Fpeu2Fu57hEGiQ%2BSk5lZdhoxhMterdx8bx%2FtOsr42BN06mtN3L%2BYvvpp5e25x0heglU1nj5FZ3q4XSvvcNHX7uajGsNXvCqOJD1s9zMTMQrrGDePIwfdJdqzHU0zRt9Ywdq8Uugcri27SvoHMUESYs1Mt5AdMRnO163ysganE4ka2uL1OXooUYQ%2FPWIIFUBNIbd7%2BS0rqWiZZmrHkfvaY5XkI0hfa1OEHr9tUXvtj63FxCQxYolG8KkiZtLv9nCNK2B4aWsDL%2Ftg2PQECa4r7WiZOni3GnrDRtAIe%2BHBr%2BbMw2giMgwhPfExwY6pgFWUhd7sEuIpAbPLqrkM4%2BXg7iv60cCq0tBJaGXa0GTEewockQbSC1ux8WDH8CkYhgJI39D5dldJS7lKmwNv%2BRWCJmrefS8gpCG24auYXQFwffSoHo9YvsGTgxBVeGaB7idaGyTMcunnx1Vi7gec4zEjgGTtSLlXJn2wZiFLzhcUBNOMWy6UhONNfB6r7maRDqo9VsO59wN2XiHkJNC8X%2FuN2nc4LmJ&X-Amz-Signature=f5b798a55911af6297e3f45b0e771a36318eb9470debb7f7b45f88261f67b5de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
