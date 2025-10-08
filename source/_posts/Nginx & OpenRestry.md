---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466333BVHVL%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQCSloj9OxPCwH2oh0K2mLgBIbkveT%2BxhOSPcZ%2FphV5ljwIgHHalf3G656pgGGZNschkR1BrvsaAqu9UrgCOJQi77rgqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFzgqEB70xR4HWfKYCrcA6NmY4jCBzTMQcPIMhp15K4HrykxDq8otq0ZYX9PuyG9oo%2B8Val0CmTSHXMZSOs4m5%2BVomWG%2BF4rrR15mNvwL1hBDKGoz1aVMzNcdw%2FztmB7LUSYklrN9iERzwQJYLBHWU5dvXvV5OLAmBKDs%2F%2B%2F8roeyLpD7d7P9gc9nAXnU0r6OEOoKbCeuz6pMMtrJ0TVsD9cD8mVweUACmr3wzaLLo6%2FBSIsgwOjZZm%2B%2Bm0ogrydkc6Np3YpXZ1kai%2Fq%2BYfl7P9q5JBETY5WAgaENh9dAqQYT1y4FJp1k%2BJ0vmYKwIkWW%2B8KsByjC4dL7vJeppsf2yAlj0vuO9gPKeQKn93Ehn8QJBLvIu40p%2FXFxQ0Dq8dasA0cp9BkODcNktsckFqCePMfIuYVy9K16OsWJM51RpTND0Y8zvEhfkGqeoTS2yrujSNpdESm3yzUmLI7FrB%2Ftup%2BUvq34oKPNQg27ejGs2qMr1EtnOnJ%2Fx0A8yXck1z760sOKX9ruVEIewCaxStFTQ45gufrRWpdNvAAEonvSLBhiWv3aQ61cPohsBsJSO6uaEMmJvJVLWHfE0shZ91Ol7hDtP78Ejuc3ehlUs2LXB2b7RQLuak%2BhrGbv1bmXzV7qZdbw4QHye%2BJ56ErMMiNmccGOqUBgxec8daB0jv9BQWuTPYrCEclY4HpzrRY9wVlRQDmW1Ibkr9Ms5Cj759%2FB5I44p6tNhT0jX0PPISvbYp4qICo7syo1b08muglPFYEwPNTKxm08tLLzs3KWXVtMpMoptWp8sL3ZYlxklNk7pDrJM%2F1shbgOX43d1%2Ba5Tc2NBHI%2FEli3gDUFORyIXbV4BPCP8Q4VeeKOe3Y8SdiUXxKGaT70HyoY3Lo&X-Amz-Signature=c2a1a23c61f57892f3b60e1e7874cb16a664f952f46aa4d696fa4bed43f290b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
