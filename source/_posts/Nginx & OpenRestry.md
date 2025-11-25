---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UOKHPHCB%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAVOJYToJ80AwPuNzgqMp0cAicRvSSUC%2Fb1WXAr3RdU%2FAiAr7GjXbe0RR%2B2qZrbcyc3CuELPYNUF9PEi2tlCAjaBrCr%2FAwhwEAAaDDYzNzQyMzE4MzgwNSIMUG%2FPep%2Bf3aJFw2NAKtwD9rRCcOCmRbxbPXGIhu8Lry8f1QG%2B39LU68YdFCL5NVcXoxCG7HTOCKqj%2Fb6PqQ4MLdmXQuKSoJ8SjECs7mU%2FOHadZA0BeRJkx4stOeW87rO1HsQMGSW1tnXTmYY2lIzBRGBG%2B1zX8jvD1UDVOFFvM0K3ig30jFyjvoBkC8PWZnSACM8mCLjvHsZd7ZqUHjR0kBiQfpELiMfxUNFcHcKa1wochIlZehdzNPPR7%2FQWP1T7Xa4ZVD1wwVv2ue%2Bt0ktxcAeoTJ%2BhrwnQ6qTE39ZV20kzDLB7f16JhYF2LDsjpLKbffut4PuH%2Fk2bLl9pf5KMcgXnVfR%2FPIIli2jCqABY9qYcBTy%2Fky54M7AC6wDxI%2BUYT0KpIA42kQnOyWPN6XosNfPeFrPXasaRTGHQzwywTRF8JoFHzecZ4oUxctgMtcAfhcUR9LuEI2bUpIwhNqGdR7uScriVqtXzKgATNMUXZpoYazAZIdq%2B72Ch8AaQMPKJs3yqzcSZ5HbBnwobk98MSKX1hOBXRyAQuxArQTeJX8nfMu%2FnFOYRlAwbMG4IIJNomAJP0p5xwRWDGfywZs7KZnxK%2BfVDiCckyGFkaPFwSOzfvnb%2BhLPofNIRowHpBxzGqwJnzmqggMwPvLEwqYGXyQY6pgEiBigg2oMXik9KkEKaG3WLrzmyJz1q3JscqEO5WoVRnTu9PBeuWKv37Z%2FKJzW7iELcyetOtdhCOvV56MhFDA5gueksn9MikV%2BfjcOLrv7VM3r7BD%2BWCXWZPPytRS2D96k3V6QCuTvCR%2F6NwuOoMTnFRt9jv0jhP5Rm2peuhsbEs%2Ba2ASIxLqA3Dm61QawfhRaClYDq9Qt3ERbms%2BEU7VC12KKUz%2Fy8&X-Amz-Signature=cc36b109d6f1c83c7b5c9a41a47cb468e2910391ceb8f29ea794578a274f230f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
