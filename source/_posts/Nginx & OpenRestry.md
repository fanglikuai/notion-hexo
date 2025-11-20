---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VMG6SDB%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJIMEYCIQDrjbSXkx4nOKv7oAjiwRidAlHh3HnUki43SVVKrYwODQIhAJ4C%2FHLAvSzFcrNDfV1mdl6iH4M9K2xCqsGYGntRhN6WKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvpMW07SF5UnqqqEEq3ANTCVhtiZ3w%2BEOufWxDP%2FHi0T7QfcAeQcLe33lGrpB42yn%2FhiYPShRDZOzRJRVqj2tfKiS0Qfn8lBJDn46lhwRvYpJJ0oe8knZtpoEvLP8wInrtWGjh0OHAsevw0XEbn7X9Vsh8C%2FgHEWzSAvxecOb%2FfcikIoWPXBfo2z76KilbUk5rdsHTRea11JXfXI0Uwa%2B2qfbXvORG3bJgSUl2TxxjTB6b9zvEUeJSKDNuop5lvOYxUacKSt8oegAaLto5WbelOFgBlp4Dh6w56u8LCEyrdQT%2BlmnTx5NB7%2Bk7j4woJZu9KUCytKmdga0zTbXdLBcQv2YD%2BoWBT14zDWZk1O9mzs9%2Fj2LeR8SjILpjE05SBcZ%2FX7kTblVPGC4BT7v5A5nEQE1HlTdjphqQFz3rxDHbqzs5McbUk1rRz19i%2FhdMTtvwSr4OEJdKcKeb%2B%2BJAEN723vPqVoVfdCvJj4mKzZkG5K%2FueENGPfsouvZh5rAsuyrW%2FtoV3kiv0M5dJFS064bw7cb4bHAm0oOC2z9gzx%2FfdM5rSPQ5IVbLIqiBBHDyXB4G%2Fxum2BN0zKyiotps8k%2BuZ7Mv9%2BGVbvT4oZDmOsubMqg%2BwzZigaQ6i%2F96ZdLGnXaLqSdwwYVaYSsz9DDip%2FzIBjqkAZBf7wVPzZSBnL3%2Fg98YInf0wvHdauAGkwIwM4p%2Bthfq9Bfo0ICjlJG4WXGxs7Z6PKRbn2pIg%2BPlK5%2B%2B%2FtFB5YWCHw1Mt4yXvG2U0J%2BSCkenbP4pKx1OcZqiYl8U0jzsXyaO6J6y5sEdkN2dRx0EX6Hxx7swez7CMFNqRdaLbOFYzV84SFLtRxXDRAtNCWXwVOBv1o3KpylTNY6ef9AMLoRwYnU2&X-Amz-Signature=1dc1afdf4dd5550fce0673eece3222fb8170a54f5f298aa316510b0773ea90c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
