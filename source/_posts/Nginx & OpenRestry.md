---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQFFZ4SO%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCIF4ULlqV7IQDL2Lm49nSBtX%2Bqr%2BuLMM%2BTtifucYu%2FmWOAiEAvQ0AK4Al6zZdLD13vKiRTtVfJRAy3Cr37cglOtUxZp0q%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDIR7fp8K6787A5NXCSrcA09GpCNSvSCyjJh3WLqIYy8QqffZelvcTR5q62SMHpQFj0lvuXj4CbrvwBNSXwC7Stvzo3PwkdzANsL34XjaCLiMHQL6YncaJ0ca1w4asHYA5kBugwe3BI7UX67XB3IaoSXd8zZWRnCs4nLuKP%2FKXb5BG23PL4cv4kJmoyCrhIXUsGBnWNqN7MFeM1ivMvyUY79PQu7eR6yZGlLjC%2F%2B2cerO1SQ9xDyJY2q0HyqDjnR2Cx8JpROs6LxQL4P%2FApd5qj3htx5DVk35ZQc9gz9rsN0YKbEkphvoiqk72pEzKSWJvD%2BqN9wIBkjsSY1n0n4pnhZQKC6kngtz%2BeW4C%2BEWt6%2FXOQCqAlAXgT5gyd8tl2VdZqEMPURYngj9rcsnr6sxpXrHopx9kKfe9uin7ZgMYr6b6oLwDkZTtPnB%2Bg%2FJ%2BhyZvFy1LvDFRXaAzcROnWhTcYZ6p4MXrY45wJdvPu0fuKqmDWy556Em5cVZesCmv4dnhkMdP6baCq6n%2BGl322mKDrizDslMp4Wu642ahtReaOoXZrlAufPTicmpUTQhmEPz9SCm99gGLQo9wTaZ%2BfstoQoHJqAFsQEP75SOPU62ZiLNhKZtSNegGJPW%2FPBB7nRdCBNSe2PzfvhPijG1MMS71MgGOqUBOj4vCsjQ%2F8UbTVLHqFsVDWCa2ykjV5ZGaX%2Fz4NdtdsOEOVegxqdygk3LEfx3DdGpylk9ZF97Xyn%2FGSzzNDds0g5iaR4IBmBJ9YW1bgwTmS6ylIugQJ%2ByknbzSiXFuuI5yKHdK8pfbQTh2usZZUUUzL15sO0%2FsR%2BevslUcYoGvjky3UoWuWbLoXncsC5%2B5GcXGcWm6ugy2QNbmd7HBsor6LM12ebl&X-Amz-Signature=2bd82bd7cf36bdb09f5ad11dba2607f71df98c078e127d61238deb86827cb4fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
