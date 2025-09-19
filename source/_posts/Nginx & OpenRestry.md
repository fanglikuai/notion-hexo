---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOLKRDU%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQDNZr4Ft9NheQtiNYAUTGRl%2BFcZ4AuG9h%2BWmyHTnjAbigIgfF5MmtdkubUsggHSi9J5XZZ5hk9idipuWC8jLbu6ViIqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJT9odh0Z4t41NR4uSrcA9ja%2BnnNuKvZRGejlOgr6J1GTjHnE6%2BJyw%2F4xYWZQbB%2FQYDADVZ9wbS6sUwGULKjYWVFipz9gmcKKnQVUM1iHYiWqqLi1rUZd02FwYuK%2FSJc5A8ZJy7EUwdzf%2FNsWAx80c%2FtbIwq2zokifrsRM3rHrLHaMF2SVfozR8KgMn8ykxUoX%2FC%2By0s9gNEuiXzW0we3Dp78uFBjOiILDfeMDelklVwI5L4EoZkHsSsxztcze2mcs4oBHx4AcDjfOxSqgJxS1SEN%2B8il0ayPkKxo69mbdTNbQ8mGdcp7%2FkA0rg%2FwOS5R00cKRQcXj2IBNi9MiYDjfGxgJ%2B3pBVMbA09QlvQpKSq0rG10VXRFt81sQvaSUrLOHvIQzHCB9%2FbFIdrW%2FQQifv3m%2FtHqY38fyuJOiSs6jN13zppR7yJ7kKeMhvv1Pxzje87oGmyg2MH%2B%2BOfuWmxZiaf57NnAzJGDsRiRbZcgOd8z6tQiyDPJ4b%2BS%2BQYfT3wnJnAr9%2FB%2FwwHUuNGFc9i9vXrNyJ4TL%2FQ1q1li64Lolox7bBtbe9Vu7IYKyLXXh4zVzpxrm4C6vmndzM5oQhmpfHHu5lKaB3FZj97OISW4Sf%2Bv3d%2FbDg1RlTn6tpRnP6xDP1i%2FrPOsOTDZsxOMJzjtMYGOqUBMshz0itXesrwYhB5t6UcCWzGVavRLNdV57NWtpAmzMCJ0UPDcr9SGUCGcQpD%2FuohuLmf55%2Fr%2FIlF6tBhFpM%2BwTDE%2Ft47r%2FYM1RLoMOLujcJTgBjkE7SpgYtNCsAFepQ2w2EO74llqlTKGGKV%2BxnAw%2Bx07wfy5gllYqJrVgP3dbv2I2O%2BD1fP%2FSNTUBDPGkrkY%2FKmVqjRzJj3GJwwjVePlE%2FJRBwY&X-Amz-Signature=51fe6d44765a3c37d10de4c9c0bfecc78255cb4e1ce06a514a3062869660fba9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
