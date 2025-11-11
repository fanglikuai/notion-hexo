---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZWK2PPR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJGMEQCICKojMqtLg8WNCjTPf%2B6x%2BfA7XP9mXYsV9%2FJ2vGXrmETAiBjALNTPRVbWeHVoSFwOhOMGHCNtdOJWF%2BZhVc%2FKpMeSSr%2FAwgUEAAaDDYzNzQyMzE4MzgwNSIMUzQapnTiAud9%2Fk49KtwDvt9F90w4IrEPUwm0vPTmx4Y11dtHJsYJSFRCG0DAeeDwCUn0UTJI%2F5IqPRSbJYt%2BOZEBIivCa3iwEbBETBxaS4NL0KO%2BpMg2zD14ZWq4Yah3EHJGqBvcuJ84a%2B4BnlmVkNBNQxH4aVzRFEe8UvGrcp6QITMd3GRC2mIseP1g7Yi9F3ZFdzySylRoqwXiL0tvNyPYmrv7qijAxyOXfKmqwIKrxOqC2GcZCg0KF2W%2FAzQEbhyi2tjWlD505E4LEcEgU0ccLRop0yLqil21YKVtb5zu1o%2Bt%2FTCzh%2BdsCBpOxR%2FwROOq3jsN2Urxjg8QeaGA3eyuHPb%2F5TRZwzHaWk7bF4fLCHuDuIteqFvf9UdjUhSdqx%2BN9KgTLAlUtw%2BEVnJ2PMXfvx%2BpAWD%2F1EgupBmr8VYbXH46EiGx%2BxmDI0LNtgE3JDjdUH3pO6f9WYYMKSZDuhCmAuzyjdFmMBDG7ZCOYAyQXWu8TJJbPaKGhTfj9TrHrmev9J86s1GvIxhBHPMozgaCRo2x71nepJpYco7OoRWwVgot6rgZqtpZZpMfMkLcZkjnAZ02%2FrEnkk3WypAmZ3gIMia0q510w7fQC1gd5HqT7m45aM7Rby4gj9g2%2FQG0ehg3HeOUdfqjG80w8cfKyAY6pgHYknp4Sm8lHJ6yk4XWRupNFxIwLH7O596cIWHATjjVmiQbFoGXxY2m5TPlMORYFzJZDhZAZc4xl4x8apye96O7EaWiOLAZe48QIiRYn6jeHGsH7KErSnOgCvVbq%2BF1g9caRuQp9rORvIv59sIk0A6SJCd6x1Zaoy05t4YJDVZyMcKXLyKRXw2dPsn35whiedKn9MuGJwmSXaTcmpWB8iviTANjmU7r&X-Amz-Signature=191c5d691b234ed9404775dd165bfd622447dda1eb865971b69ce1d392db05db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
