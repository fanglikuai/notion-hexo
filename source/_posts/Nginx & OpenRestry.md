---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZP5RGLCK%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGZ1bwyAFfgAYm1mFNXhtHGC7ZPIr3XOZFip870f7ZYDAiEAnaTJyt%2B9q60Lj458ZqiGKsgvuz%2B6ZvTe%2FZUFi1jOE2Mq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDK8E6Wt0mSJc2F0V0SrcAzqca5gyOCVccC2k8ga3heMse5Zue6Z1xKFnZ5As6xvTiO9xoYHKVQN0JWkTCxepZ%2FLvLeld%2Ff9w%2BcuCj%2FSlIKsZZOP3nA78bmjsZ7EO4BFUHZmgnYgsdiGTdohNksP6mmZMVQtEdwYX2tjtNOGF%2BjCpdHeAyleoTgp9t13iEoFll0OQNTW9vCN2JK2nVSgiB3P4nw1mz6I1OkEnXD3SEsKwX7yJ%2B8gosiUvDaoLnre8TlHKePMq9wUafEVbdUbf0zO7Bp8YFeDGPx5ne5Ch1WVhTsIE5qpFYd6nfG1D21w%2BEv%2Fm8tLf67fAYl3nwtHwpHH69ATc7%2B7CJ407tkYnMap1ztiMIUvLY%2BTbg1l21CUHG4XA4wG4Amf0RA9TJgWg8Yqk3H5cSMLGUfJsqglpjcprNVnNX5fv6muauMgeoLD7oPDVsRTwYRgGJYXPCrJWYx2nw%2BBJ2BnTUlFRbC5zNsd788lme0E2Nvr72HAJp5GWgXVIFKA3l4%2BZ0ufjODgDaYW0xTfAYBDegxLxu8x1%2BjDhUe7kOTpiokKMgdl2VHFPBCrdGkrpqnwVXv%2Bd0heXD1na1SmQdUk1Xf0xABT0utL%2B157pv7DpNazx79Ei5zSsZP%2BABoJgKNK9jZ6cMKz488cGOqUB4McnxWis3NegkNNn2K2Px9hIK7qmOsTTwkPhJ89jA9cKDCWtnd%2FFuIKSXUrwlFLEra%2B3LYMC6Z6nVBiMgjkZ9jdH6U5RxhQ2tC3miLGT8kdVCicFfp9w4hXGzNk0xpdqK6x3HLX06j9nD8phIDOy3nDDxeEOUaO%2Fhnr6z2MFfrlpyF0Ll%2FGTm05Fza%2B0dl10Kz7JXOcwMLe7Lv%2Fup2d%2Byfvrp4wk&X-Amz-Signature=fdd013e844279bb40793c0b826f6dbac0191ac521ed44f7452329dbf0f5d8fff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
