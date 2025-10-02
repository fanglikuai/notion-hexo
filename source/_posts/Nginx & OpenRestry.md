---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KBFXAS4%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8PY79fvP4NbZpTsG8belQUaqJqqh9ORKcPGI7GJq2QgIhANQCBwrDf68OnZ%2B%2B1D2j9i60q38k47XOhLLfInjPUp4cKv8DCCsQABoMNjM3NDIzMTgzODA1IgyfhYKJfZld56tCif4q3AO8rWk%2FLSLM9UWSKBMuLK9xvzRElAWv87OfKXzgMmbjGf6Llh2Q%2F1Q%2FIuxDAkYefVLz5CK3jF0sUhNZWtsL8YHp3g%2F432W%2BN%2BjNESezz1vJ6%2BHv0YC6%2FwizDaWz2HMtJY5qwCCfNAzgwsxoaPaFhqZ6uuaqb6KCNbSEqAjvxBRDypM6lStS3rZZhdiq75gerowUV6cJJ5P2cr%2BjCpqhO2XNmqr1Rioz91gzFL1dhqhdKCtyoMCqMvEJV2L1D%2BvcEAFR9uesdDASg76wunnAZoJmDMUO%2BqQcSMWhNcIXhr%2BCIsetSgX3CpJwD4M1aIH7jKl91SPSmSJBfQQbhB8LIwDifRqzg7gdiC4iVUnpkw1GYZCbXOvXWty%2FhGXFt14gumDI82KAw8oKQyG7O3niEE%2BNPVzufe6B%2Bd20RjegtOPy1UV4BHjgfNUZ5%2B73cPZ%2BLeA8bHq15oEJsL9UZUwZBNsK1Z6tslFWCfgNqKLvDBMZ9j9WqaIs2M91Ajk5RhpLCgZLXeJZ4r%2BmNazJ1ilOV1p8zKNfrooy8dxTr1NBPxS%2BFSXXvym1HE3syI7FiChf3lGZAyMkJ7ugOutg%2BHUddUUzwAMegKE1yTpX5hMzTK4ekjXC%2FEBf2ajGnCP6bjDHkPnGBjqkAZbHJ7QrYGhY5zXI9iztCBo9KqHmvPiZQDE4QiDjh4YL4cki85gf31a32cU1yilyg2a4CAU3BkNQLsAZ%2Bh9f0prpAbs3rqDQDwUWlB1YYMBLvBspujK8coVBp7S6ZhcE3WjVg5XoKPKiKfzZtwT9phy0pG%2BIC5pcwr9I%2F%2FpY9Kz6CSco%2Bb0Hh7AmMnA7kaH0kD382O2pM4RyvxCWymxhWxpyOkqS&X-Amz-Signature=92f752bc55414da9e8e684552c777e022eca43a032e5427cf99a239fd2281303&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
