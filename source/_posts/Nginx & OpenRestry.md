---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNFWOKLY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T080054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB0c4dCKuXANOCyVjhQtb7S9VqNLW8o4jgehwslMQMZ6AiB0bWLM5i0sI0vlR0XG6%2Ft6w1miFGq6bUqRUANJqf%2FCfiqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMvLgCQE%2BhiZCztsUVKtwDeoeBUxsKmQfoeDD%2BU%2FJY8h7lexoSUHvJ2ixH4RzvQEj4jbXnpfpW4PxuVhiW7dvYNgV8nKV1UDd7LFNp9%2Fe6WmLJUvObW7%2F7CEwyKKk2wuWjpuNezl5ZlO6QZB0ZN9FE2Oawfy34HZNnS7MfV3BCRBF3Io%2BWfXz8OOczXo1sylUhAMJmVzK%2FR3PV4964vo0mHBdtb7h1zK1Wm6ZSgyWTQbxEfBocrJH%2BlCHgEc6Fu6gfpvz31%2F1jFCq4FQ2mDUWCXKcS%2F%2FiDpT4mgSAZaDn%2BI77t0HVXCTPFYIctgpsOyCdBy8OlE34IyJSWPVtivbOnNgnWjbtY603tdaIsI%2FFLE%2BFZHzm9isEYP%2F1cQMWrc4ymysl%2BWcPQ0IN6mOKDaGe%2F3J%2F%2FAzY%2Fm6QDeV1Cx4aL0Y0wGHpw0db%2Bk4%2FtNDWK52cJghzXodQPMuPgzlNA85n5AqHWIluFUAJmfo2gfBV1JLsTW9dM2qQUrtk1W%2F2dYtldGnX4LFsPlw9tPtMZ98DDLlM35fw1YEPhVDL1uDV42nj1gYkhuOA945kHqF04UyLB4TUKvkmwKHs2mHvSIkX4mTADDPivxn0xjhDB%2B2rXz5wNQTY%2BvxvQc4hpCWEEGwpbEQtM3FQgarMUWvwwnLyayQY6pgGnARoJv1gCf2lBoMyDFMtTj76nPg46ey6uLUifs66%2BK437xiVjQb7uScnLwbm68CG5pB2UoyWkUvEby%2BZCq%2BBNXlpeVm8Iq5QCoF0gCHwkgfvpHxM%2BebLDsAH5ovTn4UtLyXavqqhdUXRVL5K2o92fmK3wGe375pt914SYDuJjp6YNafPaS5tEfi5jgGxovxd7RIgV3r9%2FjHl5ZJ4JenfD9bbX%2F1tb&X-Amz-Signature=72941632dad9125528e852a773e484ea11b341087067b2462fe3299acbd7b988&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
