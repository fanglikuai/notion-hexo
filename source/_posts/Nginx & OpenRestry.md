---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7HVDN42%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIE4c8r4xyTWHUu6YTP5ZYHkefrHDHa7ZE39B5khIOD7pAiEAn6k5bkn8duf3Hisj5i6DAsnl2H%2FReBHKLuEt8NN%2BIG4qiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCVG6CgB90ITJ1z0WSrcA0OdpDJOFXI9V5QvQ9FHa3SHdHC%2BPFoUarIxX%2BLP0073BoXmY2ZPEkn2nwqCvXC9YlCMfqauy3M33yZmsI%2BSiOTCkPIUdgEVueKnHjGB3tcNbdpBJw7KKw4VUjbEjGKADwakOsxNi8DIWGbpMW7qa02OvmsFIz8%2FVrR0lQLrABVM7qUWmUFV8Uh0b9pfvSxeS1rG%2BDYEBu7uf6%2BUxuaQIH830dtlJttAREdAYrMNYitUeJIj1wtIUaRq%2FXe5PeB4H8z1pa0qGWgTMhvDVPfJu3ZK3xNkfuhx7Fem9D5HXavsKR4WtyQRnPoB6NDURen2Pe076spVSoQ7FZ59dNjZXpGSbTXpyMs%2FC%2FhAayzJvZsVfVdn64AI01NVbveHTUDW7opT2Ef%2B7yKN4qQ54rZTQSjUecW%2F2rmYkIq%2F71s68pxxHUz94i2ASIWkyt5iqmPv7lkgA%2B%2F9DMTrCVNwfZT3Ixkd7H%2Be10FZhe2IsAE%2BFyCLW2DvUP8Hr%2FvoU%2FgiUX7QbGVL%2BEaCXh4qWjEas67FGI0W0W8O%2BofsP455F1t57rMc1fcegdb45PFilhMzWfN9lVi2mIGttTMchWOFtxkPmzYud6IpaEb3tGEu88wYZSq7K3yxHnCUZMVcst1kMKuU%2B8gGOqUB2T1YsDIUh7wrU1zAW4iIIlG50xi7bDVwY%2BUZP59S7f7U51ltVQg5FKI%2FJy3LAD%2Bs%2FVKE3LosEmjEX4RcNpYicMyGkeTLN%2FdmvGhl6MW6r2at7Ka8x7HzfteixN%2Bi9GDwZZM2qJRwgcFfyDLi85BqmOwO%2BLBi13X4GcHQmQ3m3poY1KVZJe1gkBFiclfzrxqfQpCmR2w3cxGAOHkkaUCszGnITcp1&X-Amz-Signature=61792e6eb0dc7ceae0bba86757f6c0b12da5673470ec3ad285e4464022f76ab5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
