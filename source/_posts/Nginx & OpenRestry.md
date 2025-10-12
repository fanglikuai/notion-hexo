---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCA5TEWS%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIQCLtTghYexVFTC0GKT2twnI5wnrJnR%2Bh82f7ol9r9XWPAIgZwrQCPJh1y%2FyUUSsX6Gi96pebCj0zO52al%2Bmgoh5zMoq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDD4SmOmzoXdPUmaurircA4paVpLbjEN9iMXO12kKYaxdGDOOKZWgtPpu5OjiUlEeVS%2B3Gkssut4FtPVXlKQ%2FGEua7RPLbWu3zZ5%2FKGe66Cnj3jH3I7d7T8ofAOmvqhFesNHrAPDbD7WR2tir5%2BBwFS%2F1y0ZEC%2BLWParSBrzyEJl4aXECzpRFNVzIW%2Fy0X4OV3PRwf5EmOjvShuGUDezu1UQ3Pzsp5N01jnb78NvlQ3MQOx2gzbmrXCB4Cw0m2jzro%2FNNq0r3rWzhRgU%2Bosg9OgvxJmmDP84pwl%2BpKr0hNoftqjPVeN%2BUhm%2BzSWOhJhTXc4jn3Xbws%2BTvxEnZ51lXQDkI4Vl2MVA0%2BlqNXJLxTZ%2BlPkudLqOV0pgcPu89xHxN%2FqRhu6j3nNRHybp2NQHaOtistLNpAAzCk0zSE66qAdPcIx76iXp9TRcndvAELsCs%2BqZHglqr8DThpGBKhj0sSnnWg7qOg0ERIrBR3JV8j9obo0FsEyvW4qOEVDJqzgiRWBfJWvB8SMHjgvl9I%2B1PvxtM%2B1FABfVVXVe4cLxZWBQR6bC39C%2BYLk9GdhtSmcU175yU9usgobYjlBSL6gP2BioShA9IceZeTr6cwhpLnCDqo6WAITmj64l0A7GoVEbDZ6Zyxjr0sKuWOHHdMNTDrMcGOqUBeoHJ0HaXoirPvIFar7XuZkWrqu10L7DiVUGA6JfmIi6mxYa4IJXyxb1BU31cpzzA3CkpNr%2F7Z4gCSuOanN4KAtzwTKmeHTvMgTGejBE16%2BrfbTN%2FPzB0%2F%2FgMwC%2FzS6HAXVq4VgnYyjR4rlsHjwX1%2Fu%2B6KQQvqmUPP3vIU6vLvwj9ISQvVdBe64B%2ByDT13rCui3H6eBvAl773R%2FWy%2FIQwAAFZkybQ&X-Amz-Signature=f419a22aa1f5aa56a53505f980c2d48afc9da3aa3593dab337391ab1d08c076e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
