---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIM54VPV%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDL7q4QgCoZdZk2YzoRXuHlIsQwHqceZWO8BWGtx73hfwIhAL8rrYJljpEfckrSKIk34ft7E44PDXRsh1Lq5dmEqOcaKv8DCBYQABoMNjM3NDIzMTgzODA1IgyhFmwBWtNReIUzDJsq3AMZFeBwIQ9FRXtllUTd1pmuAE1BrqgwIN7O61vVomYTreEZFjRZV29M7HpeM6PBYvTi9Fs%2BWj3LMWJyCnYe1x3KKbo9mCbepTid2SXfUI1EVA6gSOe1nvGkQXSU9dHLZnV8ewezRw8U4CnRa%2FvuVvfnqO7lrtqRYenzh8y2sk76s92XstMxHdgzt78xhlun%2FbX5A25J7ZWrjORpCRAdIkegUMTtytQz7P8QwGQj0ee1pQG3ODnDLlNFWZpfu1FG2XqLX9Ea9xxHbcpRJBySzL%2Fu%2FU1fwJuoe%2FhEyL7GCbqaOp4alCzAHaYP5klREr5hapK%2FQ%2FAw3SlczOeOup53kGKtM3mH03uPr05sfMdMfO733miCjzL%2F%2Br05e3SeEOG6ClTO1KQVot5rWM2unCZCJ%2F6mrDGolwC4qKSihpAV2Lk8IK%2FQkFyN%2BDnhvbJG3zZ7%2B7tRJArqnuXXitQywY3xP%2BUJ7c2mYk494RmyemOh3y7aTto4JSu6G%2BSVE02Vhw5mKWF9I%2FF4PcRYNyqtn42bK9ov6uPpnouDbPO7jqzXcjmenrv6fBGGoSqGDI0h2XPA1wolLWyLh4Ei2foC6yU4pF%2B2E3yla0zx8MrM34OUIEzYXlhkpPLcs7y9%2FdFz8jCVpKnHBjqkAYrOsewUBGz%2BSX5LPe3BlUs1VIkndCAPnQpVhF7BLhqxeGeFhPNHXyMXS5DXP9LIevzE%2BeOEADBfrdzuLo2CEIgNCarrzk5Oikut%2FwxV0aq4GvNm5SsrtyiEXeobfRvI%2BpI0ER8O0d5Okb8vj4%2FeGnabhVhgXo8%2BMe0io7uxBdfkFzCEANmnlxLlLEFunptttcmFhhJZ9ID5KNHJud31VF1mfaEu&X-Amz-Signature=678924cdff7363baf13a953d6a135e0603e19ceb01861e1c93b4bb9f8130699d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
