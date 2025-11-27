---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQ74EODU%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGarWnwl1D8mdclh77jFfWHm2%2FNmdGirUzAworzc5KCUAiEAu47XdEyQ5yP4QTHUsiri8IErgXwa8%2BojrnP0oc8YjpYqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJkbRHKbSUz3UDFaiyrcA0Pf6Ou12Wt4SnwFkInr4hEatsRsKZV4w3yvU07TiffQebKeB%2Fdpch3hNMutT6EBm%2Bi03DLCsTd71youRMpcYPuCTenn3h%2BNRljig0SCLFbmI8DhO824SdNhDv9ZKJizOTwNhibv1s4Z2SqjJ8K1cKnu7HUVvaUqJzVp6x7oCdK1YX5RZL9QGlayAs54T4kmspZIE2SDGZwaFdiaOdZbi%2FDkLq87GN2cdID2%2F%2FCBUOXy8tM%2FMphh3xap2U7lKSm6a4AI%2FPXsFENLEGbXaZDW9k3iUqOEa65O9ZyO8Cka%2B2Zd6N1euD3xgj7TN1VJJChobK1jHXx%2BzQBKsdPe1pGPfN0jr4QLmd3WpL6RYeuqK1YS4rrqUylGQLRJ9cdmTWe3sonYgOHkXTeVWnkYGtw0eK7zq2RHrCXSW7wBAN%2Bxom%2FG4ePmJnTLyikRjRH9TQlVyQ8757dFAnB4DzUU9XPq60%2F%2FXhXHSX57%2FAT9MPIlG3pgz22d4r1qw6R6Tx0P0QPVl%2F9fwiCRpMMGYYZHDftcGmjzi7KIDvTYEG1YODHW2kk3ksQErFEpZ9Elew11rccSXQfKUvfctARCLaxb549NdvqNIRc0spUP8qUdVqnR9xQHCu7fFYXgR6vVvM09MIeiockGOqUBkZBUu4NGnL06altAfUWTTSbjflQu75GAv28Jhp%2Fxh6bIf%2BsFY%2F5tWYD5i%2BdxY7UYGGJw0DleQ%2Badaow%2BfB2YiXaS49gND%2FzrBJvd4ZtccD6%2FsDFkZH%2FueqsiMN%2F1naSH5HB7v3pF8bU4Dm%2ByTOIxXX8%2B9nih0dY5Qr3F%2FOrkhHZz9us20fD%2FfyxMJgh%2FXbWZ47G1TLZ6mhZnFIZt5uw%2BDhsSgZct&X-Amz-Signature=080089f47da307a8699d189d73072faa1d933a7a358fc9a10909a8b59c20f935&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
