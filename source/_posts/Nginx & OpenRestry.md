---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667O5SHYQH%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJIMEYCIQC1Gdomf5Yhvzwu1xRJ01Ip%2F55RV6iJbBuF4kv8XgoDEwIhAPsT%2FG467da3gIYVf%2BWg6MVGTZNHCnLW0lFm%2Fs2o2qdeKogECND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwkAsHJuZ%2FEsAuXKKAq3AMw2mv%2FM12nrQgU9u75fNRxkkFnDQapN2XB0Xqgq0agImDfZ%2Bc7%2F8T0iZEChwHyhn%2BzuEwv%2BhgWa2mzbXOv6yP02D4qKsvadPbhc%2BzT4LC9zJq9Xwaa8J%2FqSv%2BlYkRFtNtqhpjp%2FZ5h59pMy9hpgOMfIXRp3Go1CNg5ig4Sds83gBuwLiCe3Xs47fnrqMjboletExDregBDiIam49mwRg56HmBbea01b0zp6Omqaiy6GEcQuTxmnnj3%2Bf%2BCF5QcB09Jsjs6UHjawVteZ7ye0J0l5dYm0dY%2Beb3C2uKSw5e%2FFZsFzVHZXLJ%2F7MN1iBbmvhtyPrZvb2PPsrzqWEKvuo2NZVyk4xHIJJ6KmW%2FTCzOVNoQMXVppW5rSJYuAXqZnLF0%2FrqWT83AAgUcXPA2lo50t4daXyhHS9Po%2BQKs2yT4327f1MTblPKxf45joSAQKmCYdTbMylomMEUSqsaiV0HqX8IO9ieokPHojdConGB1mHJfUY4ILP4JjO8Oxp%2FP3CIZ4Ibou8z9lumvqkpaWBqZEE7W7xKHeP8MdCfgppE3khoAZzTOlSiN6h1BeFv3T1%2FmhkRGA9t6r2SI2BUcjD7ehZ%2FGJ17Iv0hU4g0c2PBiUXDmNtN7aAqkXZMYp%2BTCy%2B%2FPIBjqkAXCDJRh6RJ%2F4T8%2B40KN8%2BWpezrHdYMxlh0NcM8zProWJCOSu%2FBYo61aIJKGntqS3FsyUGN7yJ52WJmlcl38JqQNt4oI6S2JT5P%2BOZZjDmaJ0tbcR2crc%2FKEJsBUJxgypLh6VJ%2BvMfFJQbYM%2Bq0FYtodenP%2B0p4bIgIeXq2mB33WFl2NqZizVJJVpvvY81I6luAmAcRK5t2amPmCCVvqB5XZbqEUx&X-Amz-Signature=07c327c84d41e915a55ef0549c5a6eb7aaa3da521c9478a08c63e95e7deda722&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
