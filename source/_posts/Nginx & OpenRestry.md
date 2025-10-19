---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RP4MWKBY%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDBNDRE9y%2Ff9mX%2BtDH7bBIYsgsh05jT%2BtRcd59IhmiC5gIhAMxlJXe5eDCXQ7omP2o8yik3EmDeF6LVc%2F%2Fm%2F8ygkS3sKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0cqQB7s39DuYO%2Fewq3AN4Y6VWssViKrKHv2daAfo%2BHCQ8DwOuhWRj2ubsTFEVk0nhQjnQfKp24MydGwvhHxwV11RucNSKHfesMyt%2BRUR%2FFRARBjBqjVfCSVnmkYNK4FdStv1AwBFdC4ISuurPC2Y%2Bz2Ire2NEr5hgQh8NY1tJZP9X1Vvb8sH7oPHZcDVsvLYehBqtdAAGuOLItr1j%2BzVSJ7BCJ7fBg9V2apVzM5SKXhZz5lICClMwHlSWCtIY8TVLzkenpLP1CWwKSFIXeHT7%2Fk2n31dBB5Mpx4zkeYnSXvE5QyUZtx1snwW86ajsDpkyu%2F%2FdL2Zxx1f%2BeSL6sR6vyiF5UQwDoCicvblxENPE5KpYiGg1kil6kJ5iWxKN6SQ%2F12gHo%2F%2B%2Fp2Mjdhzd9GRTGQqJgQMwoZtnzrlu8hfxXJpoK2CBnNOeCtVV0KKSEKlDQvy%2F8Xya27FbRCNyRtFXst%2FXPaK%2FNcSwlIflD4uGrPnNb1yU2T4x7PKwbvyljQORdNwfIw19V5AW6Xlq3XKtf%2BrmfDyT4ihlUDkv5kdfWyQrAD%2FQE522V1IorkjHuqkNIdPj%2FOctNbtmO0Dc9lL1AnsxCpHSjZ27OAX%2Bz4pz%2BWzmGs89bFk9i4Qr89k4WpT5LGGM%2FEyaYzJJkTDK19THBjqkAZpb29n2zdYJJDAao%2FA1VVN6wikV6rN4RG5dJDnqOV7y45W2iB0WXgiiui0iuSCHLp2bOAKAwVAQIwRMBUF035CsN%2BpovJWYMJgZ26TEOyjxDuOWQUZLSz3YWIx%2FIotI%2FKjBLo3KTC%2F8SjYZGnP8R3q8lgfM0IeGlmbr9p9zpihxS0f14ZLp0OECQddb63Hm0d3eSWRxTTc%2FXpMJ3L3b6Q71JAoo&X-Amz-Signature=fabb6ec070474fc9e5dabe78cd5a89d5306abea96dae956dcf6d1a3c9a2d244d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
