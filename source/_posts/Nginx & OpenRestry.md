---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCBPONSP%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDM0rSw5KeFK3JmRp%2FP%2F1%2BXgDOZpv9vVRIbjGav7hG%2F0AiEA%2FTglmc89XERhiX6RiLc7FcHNp0t%2BR3oLdgd9Ga5go9gqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCjavvI0yUaZ1rkv4yrcA60U0R3nbvd%2FMB0KSNK9AtDGkz8oFkS%2B7rbeLDvE1M6EHsQq3WCFSMzT5Hvw107Mosb0kOyzkkcNo6eIGj0vSmx2sNzxEaOfw37TXi4TAreYUrTdFkAfQL9j%2B1VOLe8PBiRZc0VwZMLthNJ93OSV4wK92SuUNJTA6MCM3RWxt%2B%2F2hb2pwa8nUbOp8MjQuYjTJn18nJk26vv9zEpjtnRQoHHjkYZCB%2BY30OkpSCBk7IGAQuYxI146cHrQHR5p%2B5qZ88SljjWe94pcgNxaRC5tZHiJ8LcNzpk3XtNSF36XITIxRK%2FS5HSouuC5UaXc5whfE2pL1xt1jeiezCZwwzca0R6zZ2zeH9m6vPkL1GDr7HNfm6uwFNiGuLJwHF0cI9y%2BmnTRnSx7%2FpGU%2FvjHWtStZk707YyDHsGCJUGv4zr90TtE3nE%2F%2F%2BK5XQK73SoNgGT6Rm%2FsT7O6T2iddHKzM4LaIx1eqXz4zz71UJ%2BN2%2Fh3VFK%2BNtNbwwvOEA5qKd3axzFn%2FWdnij1EZszJWsSkF9ecbha1RQ8NfJu%2BvmNLVMaHuxmcbISdlnuKOlHogKA%2FECCt23OAjspBLeMl%2FDUN9%2BiaEOUmCxgUD9DOrTDBz86oqETjIyDSNZLABteM4S8ZMOnU6sYGOqUBLdK8ozFRPSgdQRPryS4N9kvDjhxGdniB1ERGRBboaEDJIxBlvPficiR0tvB2gjJFuPm8AShGq%2FXhdBKMvbRD5%2BQOT7MJO5k4rYe9Vd6ryOHasfjuqAwJ4%2Bm%2BOW5iURIlpK3KYJmdxWaSZUhKXuHMR9lK6Nk4vEphGw%2BTAPDFiZo9ZEqrgauB%2F3Q%2FFJ48l3uNn1sI1HL8378o3ywzjEaDG7%2FamKQ3&X-Amz-Signature=50f365e9d107b5f598e451f17670b611dbbb0a29f3bae2a1b4e6da24c5d59173&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
