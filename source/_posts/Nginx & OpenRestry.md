---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KVPKOUF%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDIpOz%2FAgf3PEaj7mExwaPwInGkAC5nhJ6KyYhkrFY2XQIgNo6c%2BMiAkP4bqQJOavTGVtDV3GdJJtWRNZa0NA3gwTMqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOT0zDhMszvUhjaJxircAxCfXmy1EfHthQARuKEjtnit9V0j1vXUHjnucP790M3DRgmGbbBF7u6AG9bqDk9rZ1uBWdgxMQLhdk%2BUYmESFGY4zkgy72RIdz1P989Gpzs9pNr40BpGFPZeQXbLXUvQhb5SWWLdqIMdd2%2FatvkwrO4JqrRZA4rrxcXSWpD4ru1vR4s8XZfDo%2BY%2Fp1U7RVoHdY42edU3xHWl1INz86Jcw%2B9bDG%2BJifhJUDF%2F6v4zFggwohA4bYxTeTVjFkB%2FUBh0GUvjLhzdvgPYMLF9G6y3VkCXrTTNiUxIfTvP4R%2Bz%2FjhSF%2FBN%2FzsnM0Nbkv0XcXitM4bTnouvMMMzNQFXqsp0Ja1eETprbExWaIGy5DDRyYB9s08yy3klGDWYHD%2Bqz6LEdMdQYLe%2FqO8853Zpi2AfA9K7Dq%2FCxnOG27ph7zrSvJSX2BKLvA0Ak0w%2BgzCOOYVl8Yebu1rDXaUrCPO4S6keNWIEy0CUbfmY2au0aWcoOSvjyQXAEzSI2kNa%2BKtwczm3MyJrEZq5cqITjTvRAxvWJQ4Kr%2BNZcG%2FMhAyU26pI0UWSyQxn3QfoebSyFM5mRrF8SI5mXYmWByoxbHDnPqVJyQCQacwz51mJxeFQ3A8txjhkRi%2FoL8hRH8n3JaQEMKmu7MgGOqUBuuNBmAVI4C2YaPO4F5JdYuay6VbuvaRfa6KyRavCNk0zzIGCNR6X8qg3xilWJG61THBNVuk2PG5oXAscEgRqnxB4QdMyqAcaY36GA%2BsIMB57DFDIFT1lvEo4QfxvSfHKA%2B0j0J54hPVWbmL8WUy40xSGZv%2Bi5%2FTqsXrtAln5kgWFmeTKw57aUvv1kACG8j7GMCG5hpbmPCK73QqNLO9FzuzQCYrI&X-Amz-Signature=b14c3105fb6bf378161734c1b09871da660c71ec0710d2593cd4cfe074826948&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
