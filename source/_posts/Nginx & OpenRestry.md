---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTJA3NCI%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIQCVn6JNTgZIvEsCWlL%2FgnfMKo4fPMm3y9qtK35XZc9K8gIgNZvTpVzpNygTCdWo%2FpujB7Nkup0hP7ADRq0OlfCN7GkqiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJQOlr6UQEvC5ZCbnCrcA6KOj920eG5tyX91TMA3TZuZ5k%2BogiWZtWh45evXPcBVCEKUrR%2F33Rto11P%2BFUZKBXhZG6pwUpaZ%2BPLNIMNSKj4abhZ5iYp4wMuOqOW02BCcA0uODnBugILZgdJ%2FHi3f3z9pCbs7X70fKgcHcLOK%2F5FKqHfJb1wiA4blaAChIR%2BJ%2BngvsuHAJpafwBjUOTy63lU66%2FizoOKtK4EWvCdgtOZ0ywkT%2BG3w5DHckuX8SN1PT0uyLiSNSNnKOV%2B7i%2FyLhtizJM1rJscsWJaY0CF8gT6cdEFgE12PEMsCWUnK2PyeQmgmG8XvrcYF5E0J8x5dh54NZoUKlLOWauwMsGMzeS3xXYyN%2B0NINMJYJruPC%2B01jsWBGydTMlAYhX6DyvrG13b%2Bvq8WIs0yiO%2B7YxsLY%2BQJPpTjNAEvtjw91FXkhZowmRBDC2pGMTj3ThWz%2FivuSTJd21dIOR875%2B4dZ02biLcWF5YYVENS9iinQdLU0mHQcSJ2dzfLAMuMmoz4dQwQ3mAjxK%2FhVOOqamNBVC7jwTBzgx1S5hsiGj%2FWJwxaYYEHX6FkhkY0MR1bV4jVUlgnK0%2FXeE2HW1gdhttIDLhbnhp9z0xGZnl2tTKfO7SPnYP0ii5bxaK9o5AYQAt1MKv4qsYGOqUBRLSfRbsx9q4fm6M26DSWDuNRsGt2tL5VbgthlksXH%2BunZUu5YHAwxHAmlHgjokgxM1R%2BTH5ehC7su87HXDM74%2ByT9tfzaDQARZvck9hKzgpkwD%2Bg74aW9sH%2BbN7xH1i4uTjyttNNHq8qed4m9fU%2FYhPrKCoxujlRCyCrB4%2BNUxUqTsOi2A%2FzjvCEGX9TOTc6oB2Mx19xKnVYvKMmyUOAhOLEQJzv&X-Amz-Signature=84b95af211734f5363f33532f278409efe9241af503d7d1a85a386d40b5ade91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
