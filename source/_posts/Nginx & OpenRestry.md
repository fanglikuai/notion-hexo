---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYQO4FBC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIGE%2FrWg5reaNJ3zEZVbsJf%2BcKUqsYfJpiBW3rj5qZgmsAiEAsUOOTtly78DoM3a1d9UbV1o9Q4ZtoAjEQP1tQNmirqIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbeU991zjo2h2jFySrcAyM5cOBTwkHNZ%2BZ%2F9LOfPYxegOVvZGIJYF7mH9A8QYjeTRUSl5P%2F4v0sCb715oO9jn9SH3yyD4FIJVa4ir78RXzBSoeiuEUD01xk623haovRx0Fe48KsNvJR0k1vhSkxdb65yQBs17mecvcnm4C74lIEJedg3X0Mq5Ox6PZhnlKCxyx%2BmBpjc76nhR1Hyy%2FWznb3k3%2BRTLwAFFi%2FN99wn%2FOgQ9D7ugfOdfH8umQ7t1K2OQXbXX2x5eMZQsyQv7QOjq1pNSU2IyyXo6H03srkDh6uzW6ciqAvaT0tQth6dSna4tT1mjPvPxhpHYvM%2BvqaC59koyz4GrQATIeTWJMHfVJU5mkxu4TiE9EnCQh4Qci7rkJnKxWcZMLTpPPSgqXtbIZGISVUfxdJGUirGTPR5iEEHcJWTQ5IWbI%2BAdEWQLB4uWd1ggbviTmDI%2FATc%2FIXbSGGq50D%2BTYQahMf%2FBRjluzJOLf7YcHFvFSd2ZcqasmDVCcvWoPYelVWFiwhQGuC4Y6pf016ve%2BLF0HiLFoOAdGG0cUQAxFmLsUjHqVJ%2B9VPrLlp6ZWbZSj9HCZH8W%2B6e4zDKJpNCGV6Sxe5udN7Mork%2FlscE%2BuKknyT9A%2BX0J5Z1wwOIiN19R5t4rU1MNyIzscGOqUBRcz%2FJvFXJ4iyIQX0T%2F%2Bc%2BTuI%2BQjEKO5DddotKL%2BxPwxfrSSwip%2BSUNm75Z55XOQ1XA5rrejcFVPp6SIUE%2BFr7dMl7drGNJoUOR5AdUqjg3E2TVDc4VGV8kzR14ssMpRC0n0zOBPLWuxBm5pFeprIIaut1IUJw6LkubIPXB%2FiZXgo%2B3C7em7psbfVJhtCvP1OPsq1DDXBStEKXH8wjzM9p%2F9TiAma&X-Amz-Signature=f4c3ff161b5ff2c0a2aaf1a9f25391a05d291b149aea2cbe461af608d93cba37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
