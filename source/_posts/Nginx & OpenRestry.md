---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM5XL6SG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQCOujSzxtEUyDhsQ6vs5%2BxikXEia3wsejTkOdiNCu9bGAIgauj4dAe31bn2p2xWtohsecbuMwnfwRqTmpyEw0ItHG8q%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDIG4DQ68s9LLFkVUWircA2j2fOP0aQaxGZPU1jUBnTAGH%2F%2FZWoE%2Bt7w2c6vz%2BdG%2BXpt5zS7elTbQ9WZY8jPkmLkDcCh7njoqASNqRN70eV%2B7xj0cRZbimT0BYimUAYdE6JP5fC6v9WZgJWrW25Gsn5Tu241YO3qzH8WR%2FGQVNuD78je4FhW5xPKqXXN9anv21ithkedst48kVUxg7dfXKI%2Buw4eH%2F58%2FkvCMoWd6IL2Afb0s2Bevg5sBmGIIwXfgD15hjv%2FdUL6L%2BVhqaOAFDfrLF1dH%2FlH8OdzzC8Dvivkwizc%2Bv%2Fu%2FksrtErslAnuaOCzFaz3cnFE0grYD06wYLBvt4%2B9CEGagJrWFuIGyOUfFigQqBH12ZiX2659mkWqyMcC3J2M%2F9kiGpJaes0aCri1eodgvunx7NIcXVRbcMPo6nAJ4ow%2B7JY05MaiRphYbCI2SluBisf7U4z0bDq9PxM6cxYN%2FBUtSMpnw6FNhnRtNFAxdFDwlMLhI9UaZVgms2OLpYLcy8ojJ2wylX%2BbCuRvy4Zx5PlXWlV%2Bvy3%2FjI4CbN4G4r0TY7ZBNXB%2BgE3SSbDaGHyO5xqv8ScvwsrFvsFXq0uiL3PqXE8WmtF1ZR8BnXb4Id0wig17Tgy8ha0xzB4U2sCglyP8sUR4pMLGYxsgGOqUBc3lwpWvrnKaBB2a3NAfkZyqoOW4QYbLSfClkB9HaRA0psO3IjOFepxJLlJO9kUey2mEu4ceysmP0t7E6rwDLs4ZaNZjbH%2F%2BCz2mZXCXIE%2FOVDrWWMChgzzISFXvi3m1%2F8Dyp1jdvTlzha9wUSRt4NIuTzRjDt4xnAAPxBWo6SbmxV%2BaVkcpUvdp5%2BFdqbyeFJ%2FbBMqeKswjgRIVDPvmDxbjx%2Fhsk&X-Amz-Signature=a5f457b51206cb71a5440dd67444ddd1e0cf9daeadcd27735e759df21648cd24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
