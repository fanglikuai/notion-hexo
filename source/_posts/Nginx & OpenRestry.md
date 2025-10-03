---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTLAAY3L%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC0CKMGOy2oCdC165xfiXfkBwkrQHoVBPbSpY%2B%2B%2BO%2FmhAIgSHOVhDZ9BcHcpbB6CidwKcVbyohCcCsAwbeQ%2BOHc1%2F0q%2FwMITRAAGgw2Mzc0MjMxODM4MDUiDGEgaYZl%2FVK%2BDD1YySrcAwzj7CGFhO5WK2pmUmi1Jla646MFegnGZnSA%2Fywoh%2F6mvbWpuJwh2QGM6cMk107YL3GCSKVjDTTtBcD2GNvbQ6NtPb4Hh%2F9SKA1t9aTlN9W7ileGuaNLBRS%2FwgdVi%2FqjIwTglpclWUJIieXoTmyuUyO1nmig2RtD6a7Og2uHnydG3Ddw4Q2iDI3xhNba4qyWD8QWQlkjNsjOvM%2BhKqUZI%2Fml3kpPYLJVjqdZcynJGkb69i6fCxjSsIPnSBNrIqt6YZFacWM0ScE2QNeYoxKzYX%2F47ghitVcJ8FD%2Bq7Gb43mKSsCuy4zHrU49IYxffLpYPXeH0xxP1Yn5JaR3k%2Fabdw1SPd1yIaWoaE0sZFLUbJrpjwI5PdapbTo3ExpcbR9iPlQghiOy7rt1RoWvQhN2doaU22pWKmgJrMEEKlvl29lPJ%2FF6W%2BKEc49%2BONuzP%2BOPKFixp8qkpOCyJ20Lq4fE5up3a3qz0iowNapjoySExrBdDZytSeEYd6VT9PPWF9yA7SS5uPGuHm5fekH%2BR3TD2nRIbKuZZF6HcZgzU4bklVweaw8sKAUmf2uS0Gz6mALl08Jlx9Ijdf8IqF9xXGtN1yj7q%2B%2BEbkfp%2BFLXkMvON42Fzl070DEUa9fZROZMMJnagMcGOqUBAgU4lqX5ZUDvPTponGdnX2LHXThaYt%2BVMJN2ePos1LvCyfmwqd4eUW69y6oMQEfrBUnL3pnbIyKIiW4rFlS4Szi2HSuItlJtmnbs4FgCSehlH%2F3%2FSM6xkhi6D5iRXg%2F3uPHD1OmoNL1hWMPc0IxK5VlIBEha0OvOAfEUEzsni5Krwk50zj%2BZPpEseQfsXLuJIdhy5nECSbT62dQDq87QTRBf7K9o&X-Amz-Signature=c6cdb10f69810c59970413545cb752da02cc64ba95080c4ed325c0c7221ba824&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
