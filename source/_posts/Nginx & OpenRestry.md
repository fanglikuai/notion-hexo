---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZH2XGHA%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T090140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCICu7se%2FqKEZGxrZDNnN2RFSEbVkNdlKgGxHh%2BvNcaybRAiBEorfZqLKYdLUdpzRAp7Ac%2BqXNacA9NjhQU2ZEjmngniqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMljaLBpB%2Fq88kseVSKtwDNct9FZxQZWS%2FAC%2BoWeLQTXGy9cJSmxTQE6AOm3rv4m2jc2l22EW0rDCmoRLx6%2B8xDsSqNOtMyVzaJaWrQCVAuXzr1aPvqQd1GV5wmVPHDyknNCoBhfjye%2B%2Fwf3%2FOzopw5cFmk5pdw0wnIZYo%2Fb5ZEMVGGTa4DvlejNs4RSlp9Fv5J2bxkSK2%2FCZdj2HvIjJOQFnp1zEQl2rIpk25quUbHpCjx9VYqwAJicBc1sthDMopT%2FUt%2FkCU5nsKivwIfmdszM8np6el6OmBKuZqNlPfs3P06xnHZlqSNe5QDvDf4hBdRIBZDlk3XQZqLUkBlI4ofMHkGfhszFUUY8ZgD2oLzeGUjmH%2FAsLbHKZFu6tR9H5xxO8U03G%2B6WWGC%2BoNeGNjoZFWURvwBPx82JYDMlaNPZuPGiouFKrc1BNuTYbX%2Bygy5MeFZ7BCDcnDr%2B5lUSAIWt1uk2goaEsQjrce%2F2jGi4JCFnPoxhmHDK7nRhtIJ5640kOvAtt6Q32Ip8%2FgZIk8F%2FkXv%2FTGKbYr5CZpXyoemsdidtaWvI3fykVI8mAlnT2n0orbAknQpmG28kUjeemobBtIlEPC9FFqjqQesFp%2FEwlwH9AWfKHX0Kk76pKBtqxaHWFyBCcVqfl%2B%2Bakwk%2BKnxwY6pgHOUe1g5Osm3%2FfQvWNRCnYyksUDBeZvBCzFW1V2BoVhzBUW8K%2BgX89ZDecV3RJRR6i6zhdA%2F4QLFuYCXAHTGnCoqGBUUHX8tsGsNn9TRZhwb2%2Bj6WXlkN7wtz0nxNq%2BgvyJmAeGww0qpC6HrrGonXSwxWw71qTyfW7uutcplzB7b5Dh4UVlXSP1yalWRpijZF667W8ZebAjADwUhPrJhzvVMbVYHQqE&X-Amz-Signature=c5ff520c20b953083faf4115bfe18ad3a41fa306ea2d93747ed4582d5c160ddc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
