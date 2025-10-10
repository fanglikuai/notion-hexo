---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666AVW4OHE%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T070100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDv2%2Fyek8DON%2BN6J5%2Fz1bXEKu1V7OQ5UOPXSt56VyIGmwIgCf1wWVfwc%2F%2FxAdkaYFIVJ9C9pLTOubArYt309khUoKEqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOSVhYxWJU6q1Fz5FircA53hvJm0RZ%2Fa%2FUT3JCPLtfhapYLjalOsqDKeB8FwrD6MgUmgbls3Ng5MHaAdYALm4nrI%2BNjRRzDS4q7KDYlrE1Q9hmbGJXxEdQrZvsT5tyLwTSPCeSqa%2FlNxv%2B6LnzHQ98RJ8%2B0rbuAjd5ipgc7evZWxJG8owJ83s5A0LGA7snxIHJ7sCqrcKinJ6P2O6CLHvuyjPYBBwp295zOtpGZaC7BY9YWPTYTMZlQGitp9fxAbnIqAn2UeEDFkwbKhM4Ej37iAKlWGW8xtXFLpTnY6v2W7z0HRlIhtTQlflmSIFuQqPpIak%2F%2B%2Fe0xu1YP0XElr9v7NOdEA6TMCglYZUVWVXUTYiM7Pmo9FMDTzIT7KGQCsaFzEsbVsxbkEkJB%2FciVgXOaRDjjwLwH%2B02cEFXKN7mAXYtcLmj3gOC%2BAVZm95Ok16CLzu3vXVFwU0kyPQTXvxwVze93JtXxTNCOx%2B8z37H6cVWEVtF4s1zXeJAGOhNaQtWvGAi8nDlXmyU0tFXKvrp2tQilTJW%2BlO%2F9tac86nin6Rh8mLNbvf4Bmeb2h0fvpCZGavhNrkoou1beP%2FxSvEueKCMCRtm2sL9WkA6yTsCmRRLlvKWc2jONOOFW%2FSMnr1i90W7rELFxO3gnRMIDaoscGOqUBR7ZygwP3Alq1FacTXD8%2BzRFPRP28ahA6PqBCQjiIQMAN8yKCgJ1Gk523onbhd3GaR0Qw18pNGbr9zo9kRh2BfuJi5gNsLvUyDM%2B8Hmb4860BzYBMHlIbCZRzUtxFp4cz64umEtqfZks9iFHsrruqjnQ%2FMZBFy0brQhjdsKo8KZo%2BgHK2aT5tgJrzGhnmbF8Bg5c6jWgI8wO55QX74y7aW4niBbhs&X-Amz-Signature=dbcf0654f8cf764fa598e88dc01fd697d46f20db0f6823ab88908737ccb0464a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
