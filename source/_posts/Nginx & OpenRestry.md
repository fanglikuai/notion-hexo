---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LXZBKQY%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD7JlhedcaUb2LqYRjBhtYcKGZ%2BX1%2FXp%2BXZnecLmV5%2F9AIhAL2MxA1K5%2F%2FuZN6Xk2Iz8wSY%2BJyVeV%2BvG9SwyD2lzWuwKogECLD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyPRo7BSLT5pBuWJQ0q3APu%2BfgfpjiYoldaXfu3UHrpbdXvKATJhcFukglNhUu4ecT3T5zf8GWFpkS2qoQjtFDl9JlsxSM6Ci24XNeRtto229w4xobHM1RWx%2BeKgcv4nr9igK5780I05buPogAF3m8PHTqzTOqpxUZnXKpDr7ThLTeCf6ZTazPDGQtLJNxyPWV07cerlWOQddAtCw5xnWLoUeXO2mOTEBYTmhnuRQ%2BAUSF2xqEhLxs3wR86bkmE6EGwqPaUrivhbmMH883UJqovZhjj43JZN1SzBh3liFe9gcVmEO%2F7AhmkzR7XBVssU9VIhTMyujd4FoFWIWhmjKrmAjpfvFaxXqaRmmyuMtPxMVTq04%2BiG3kFh%2Fr0bbF9QC%2BhjwKVCGDCVPn9fzQiC8nkVJ0mo6mVnFGkjvG%2FpuuDEyka9dacmelgHLwD25m%2FTvlGs7duA8U%2B%2ByKeXlnWmLOgDL5FtA58EHxCwvI3tJjf3VgixWAHCMtK1LBAVYYnMbLfp21x9UaoTPPdiHbFd1PaJdw077RVAA8zVavNekGKGbtASKln35Aq49m1UaV9p4SHevn22Gs0jOTpmRqe5%2BEqkzr9FC%2B7KdPIillV9PSnKST8vfqfCPimUFPxBzy9EPdFnPByk5ZJ9mx%2F1zDKxLTIBjqkAZkZArA3NTgD8WN%2Fca3cwUEMJArKuBACP4U5%2BwkHHsPtWTTGJDqvoU9ttTs%2F42uEkbX0ePGWXkYy4bzezSDprmkDrOdGQmOFpK%2FQX6YUv4gFkmtlk7vMgLBANA0HnjlNOBZHuq4n4bDYSSM4g4L4PY%2FlCZD1tQeGnjaPKijIibicGWy9a%2BFr7nV44xUthWYYMWkyUwVvANtwMb4Mlv2BhVdmTfEX&X-Amz-Signature=64824da5f54090a1be76b2f6b3f690893f175a364cdaefa0d8cc4d4fa2a2c017&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
