---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOAO6SSB%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFR7tMqMnwA7M0YdOOj8a4XpyOtFdCBN15xyOEfvWzHoAiAfRqfNoLN9QkGlVldbutNFDHl6bYNLEPNPFn5p71msvSr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIM4tG4PQgMnUYs4CY2KtwDvBIz9NvOANP4ncToc0UX7Pmo5SPyRBcJsNsFrhvHNvLtH7%2BWY4c7KOksAGVxgp7CAIcqmhf9wnWvA7V9H0LKs00C8euF2UAewDo6X0DveMJ%2BuHD1%2FUYYC2cahVYsgExOEpFwJxHtdponnO%2FpYjELBFgMxLEUwTiteb%2BTQjKQyDjmemPa38Utq9ROD0XcU5nV72YohbtQsDjTCrFH51tgIhe1q2d9INpOYpqlckpcqqZHilTdH9XW5%2Fk7E8Z03BObftDLPvQVGRaScKOBYpGa5QyWVDfp5nXV9uCzKneY%2F1ubnlvi6ehE2OK4ML6azVG7Q5fFOI8zLjoWkg1KrSwHI8ehy2fYpmm3aJGD9YHrixbstafCJzV8xrDZh91wfP7RzW5NoSARAKLO4XcBmW%2BlV6zh9w4RSHoX9cPtJuf33SCs3eLP%2BeWvxN9qJpSFn1VH1iHt53pOke62PdgSniirHHrWudKRc%2BJPZlm3nzrxrxUfpV6jYLNgQX0%2B4mOdccGrLXQG8hmFZwv8UrutlwKTaxexWmIvoL8bzvpdZB%2FtSPltp4BSPGXl09KQDzzBacW2M3h0%2FX1yH4Az3ivSDUKLldg3Pyvs2DH6ih7tamXF5h5nUjPjfHAwQ6W5Z7EwrOmQyQY6pgGRvF0Mom3xm9F6c8%2FV56jmbk7cdeJGH2lspIdJ6zHp5bz8rsxw%2Fhyv%2BXp%2BPIFnhK9VtxVcyXPD%2BphuA0TgeQcpNX6FdUmaUoTjySftuB4ITJUdwExJFX9%2B6zRcHAyL2keFrvJfhXTs7AzQ0R9ND3f1kRHRtcExsx%2FO3a0tF%2FosC9cvsU%2B5%2FRxASEdqE4td6tVVm2i7l%2FVAyPFDChh6ZDs14TnQY7td&X-Amz-Signature=c1aa4c52c081860e7b3ce360401add436b5c61c8606bb4c579b7ebe5d4c4033a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
