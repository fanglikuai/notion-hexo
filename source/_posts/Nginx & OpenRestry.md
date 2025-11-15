---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46654FFNMPZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcowijKpCxXGn1inqZgfGJ%2BGKjMPk%2F2TuJADjB1anX7wIgdLul6ZpcoMtxH1f6vy3X3JpoheIVbDHWZqjrwl7PnEMqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBN7sWTscKE70L5A%2ByrcA46A3MAIuon8PIuXDOdzOl8gQ6KuxD8i5c0UeKCxP%2B2qWsvIleuBzO%2BwoMhNJOKr0u3p%2B7FRpoF40MuQf11FRjR1Lib2mtTQ2NJ9q6ubyNtxwW0ZeB6T9zY8BUW9cBLwMZCxFDL1h2N5ayLbi57ITnBbwDHplvxyP6muNURTilZLmGDemSgsdk0lqxR9jXZGsfZhRmHMeJJ25Y5vVH8kab2nFI39AoVlPb37S3it2VBwpkuYcHmen1s26TovrVAxsodhgyRZt941zwvPCeBwq%2BT5VWTEZJKg%2BgCocvm3jiNw%2Fvqp29eJ7oKNvI%2B4%2Bgb7zE1ARXDHs9AOF8JkjujDB6DKEwWGGZGqqDFcxTQ%2BpG4b47jjT0p63yGhWOInCX6Ki7ooSzQt0zG7T%2BUMrIKgHF7KEirIR59uAHyT2201ZWJT46XDHMquHgREj4I44%2FhaWfafi6FOdAZCYNM%2BaNomahWvmHWnd2iAjjkNZPnZdHjyEhfnMAX0HAweXrnCeGRnjU%2B%2F5i5wF73pdjiwTU17%2BUdB1aMUVtObGkYHY%2BkwOeed3YBM2HieE1H0El4%2FcWx3fvqgIW7MziGoRAIWfIAkoZ6OFQg9h86BesRBJU9pJcZuip50Fhx%2BVIQ4JHheMKKg4sgGOqUBierQjN01km92dsFSFwWOX49d0Yzb0aPYuiyRRdt9u%2FlNr8zoEGJTlH4ljC3UG6Xj9B1g4WSWGUDRCa2ODXsMmaQfXdOddsx5CN2uYptgI%2BixsJ1WJb6XDtH%2F5UlsXks0hkkTvIn6S9TtfjBkjoPAqj4DwkCk7nt3qdPzPsQXaEKm06%2Ff28q6qSs2%2BYGzyph9V9rWOgMv2vaTVFHjE6ij4kAikpJN&X-Amz-Signature=0ac7d4f9b94c0c2ab35583811ed130377191fdc33b06d478ed3a774d041b4ec2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
