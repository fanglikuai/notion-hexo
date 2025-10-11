---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ADBN5RS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQDtU%2BDg70k9h1vveDG5UJTDdLHM4e%2BocudJTrk3vNF8%2BwIgMSVk9v4%2FHFCtBkK2Lgww3%2BOIxaDTJGIdat%2BUnZmJbEUqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLuDUU8fQizuuc5XcyrcA58PlmUgBPvhhxNAu%2BfH%2BHmN3LYRowbHXkEDRmQy%2B%2FZDPPUJ3RMweg1QkyfxjtsQsbMKGnEV8prPL8b%2F%2FmaB4l%2FWk0dCiwoZxMxnLnBH%2F%2FMH3QU8Up1%2F7X2nXr5WZJaeC1gcgfAqLnH4f3AVYVlcFWiZCzZGgAN6OcfAbsgQ8IvfvR7QVMwTa%2BC6BTB5T9bYGhQX5RAJnsQSKP%2BA89E2ISzuKY52UNPFJzRuvpV8ntP5rTQzGT0%2B54DrkzadVo6z5RF89HtGaMh7V7VwgxEaebaQoTSRYzlfPtZ1NU9qdfwCrnMxty41Ge44oh%2FVofmhKhmv71LHV79bn%2BmHR6cGz4iE9DL2Xgi9RVtRTrQB2TZBVxz5H2xynpOMYZAgaZB6T31ieyfVTMzao6VWe3jKgV9c1SwHRcCDBZ7bwxly9etmGtESZQrf3go4uhACAyEy3O9zql6mZkI%2BH4ElcC6lQZN2vrXvb2PCwrKCnuCs9M%2BShYB0aMRTaTwvwZjmTCNbkQcrfLh%2BHFUtN6I%2BY4U0iFjAY%2BCEXojdCWAkpfNBKKibmprsRC55C2hnySaw84ocIl8D%2Fl6M3GK3m0ZhTi8YZayHYBCGNfEoarKeRirohnoiKib4xQsYlANhGO79MPrip8cGOqUBexf091CX1ocIk5erp%2BBAo%2Bg6dK5XTF%2BxGzbinZA%2Fxzof%2BxfKz%2BtU6g8libOXS%2FJymrP%2F6cKkWRv2kL5a63Q7Id62ntnWltYsfnY55lU4%2FF9UcJ1KHtC3L0fwnGUx8eTchT9chCdrOOA%2BAyKsSKJOcNd4Zcfd2w43iv5HSRtjwOsoUMD1y2hy06PDNy7q8oyASpA8bMH493SDErJ0vYkZ8QqJ0KlE&X-Amz-Signature=d30bec38e5a86f2ecb2bf2ddec7cb0b48b362855010bf5eaa04686bc4e2bee84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
