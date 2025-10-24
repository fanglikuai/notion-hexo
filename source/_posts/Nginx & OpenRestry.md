---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664SM6LKPK%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDu4pf9jjgCbe4XhfYq8gv5OYTQbfhHx%2FI%2Fjg2n1yM05gIgWKYW18lg2SjujrJ7PnD0scl7MmiSQlu3YRtHGz2EyGIq%2FwMIVhAAGgw2Mzc0MjMxODM4MDUiDDhQxaTCpXrHuDyegSrcA0iyfAxxjOwBJ5nRSVamU7XvJihGNkTJr%2BosQyCLmyv%2BIM%2FCyALqh%2BDuccX5fiCx1lp4tFkV34Eno0MhLlH3XcubXhaWcuUa780SQfngpRmQH34q5NtohNBRW65KV%2B0fjXr%2F6RIkLnNxgu%2FaNLh%2F9EGZq%2BYdSMjUd5%2BGdjmziexBE4C2yvuVRiisFNjak8xWaaluDB3Z%2FUa0SP6iEzBO%2FxUhswRMmRm3y1XhQCdPH%2B68GLBFRqXR2YG5J1LxJaSDPAbDsXzD5VnT029ffmwxaJNcUM7mTA34z5in8t0wCkXHVuXaoqY%2FEnlKO1KqIQ1mNyXcsR2wgmfmnnVN%2BINyRaZeFbs9JFX6Dp6eNLCIzgf2PAUzGsdfcTBtvRWQEnZkH1xMFx2EhyfhOcPdzdvbWet9cB6qPotD5rQOMme09cJDiHsjWG%2FV24Yw4ZcG5JObRzT0JLh39pkmkMi6lRxD5Kc1xvaOZHwrtwvoEjK%2FIob5HBWO8i52nA%2BH2wfUVEK5NLxoe9fA%2FAYWstfENFq17L2gocfXRBaIgFgGr5qSkLUhDxZburqdmiNRTK%2BqUljbNiiovKLsj7Y5jrfupUPGHO9cBonq5RH7NrxZKqcARAxSIlara11kjMWM1H81MMeL7McGOqUB0uJFZml9CxzU0C9WCsd0fn1T8jced%2B4XtTP%2FNGatp%2Bx%2BseEffmkBlzmDFLJKb18SIRt1VWPSedpKeCPn6kPA9UQjJNA%2F5p7USFVSo8tiwoGPZutqyYyP6ewT%2FMN8TgWICyQG4nFlYbz62gfk88UwqhEEESqztLbUrzem80YUXlBJ8ldUZ0MuSxBZGCHrPNp%2BsJlFjrruKMnCK8EPJHgg9HIq0I2v&X-Amz-Signature=ee4532ebb52f95e858a850d503d75a9a427d65810d747c9cd0d698f006fdc3d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
