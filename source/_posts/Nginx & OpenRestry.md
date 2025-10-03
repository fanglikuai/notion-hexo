---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SU2GOCTV%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFqhl83%2FjoO%2BJ%2BVWhgURuqCcUUYMcaHsooh%2BIX9PkQnFAiEAsDdipTvNeM2KSL%2FMmPuI0PrrJzIbehqKlsPq0NEqsQQq%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDEVzYSNpkn0OEkwlyyrcAz%2FhwOkBwZdNeIgYCg311qtM%2BSDZOV0cZw7nzAQt13MmHC8wYTIff2WaXBsT%2BEgD5ygWlmpzr2ZotiTCSuqlJF5UXILaF0BSrVS5qX%2BuWINVEZ8gqxs7CuH82xaKvfbSkY5EgWUaJLjLOjy1y1wWEkZOH%2BrYBAZHGVq%2Fr0rpU09LfX1GaGZsQJwTTrANlmLSCyxyVJLbaWXcED%2BTfnYDu4ebOibuvlclVefry1ttHDBFTn2pepPxd%2FBv%2FgwylipdN6JoOfDtBM3yQ%2BSu6kTruU2tegUjWQ10Z70DdkacB4eBLvHg3TlJ5D4kluvRtvFAYMGxG7Yx5kfYXWqrRzhqNbxWQDvoIgOO%2BhS1QmA%2FBh7JodM1ODGcF%2FsDdNULXAkEtSsmct%2FhLrHG%2Fjpa6SKlulVxNZ1uDgrwLVkuPY4KcpI%2F%2BQUVUZlzC4lL51EOmqI%2B%2F4sw5%2BdXBsZltCr4VPKXWIu8fGKmOsaQ3jeZrRhOMdm4vDkniIx0oS22dFv7qJdHK5w1SaNy%2FyDMeNGIu45tG12Roh0d4%2FUR7sfqbESA37%2F%2BIA8Vm3SjWlUl3T61ckU8tirKgvoeV5FV3jp3PhSSmlMHYyfwLP20znQCSnA8q7f1CJuMZTS%2BmX2X2PGNMPu4gMcGOqUBqeLahfVivbpolElVQ3T45cf0e%2Bs0Kg7%2BMrS2hAdxaI1lHH296KcY8oCj7o9lmufHrFQoHmncBZ63X1La3ViJ5QoKxacvGAX1YhVjOLwFuebX3TqWl4e3Z9iiRqEVvU2vB16RYedJtoqI8u67RmC1gIWh%2B%2BXD8dvRvewxnpiuqDDyTI1qrC6998JXnL1P3vgOm0a1Y8Swi3xa3cUuBA3dU9FJfuaw&X-Amz-Signature=10dc4315fc1af48378ccc83108a349a30f7f85322cb5a5b61db8530f92f9f75f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
