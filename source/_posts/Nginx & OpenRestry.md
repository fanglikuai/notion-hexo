---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ5XBW6N%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T230126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGHnfpSK%2F7gLX1futJUkcA7DL%2BRW2cGFi0wx1Cq1tYqWAiBrJqG%2BeQXjwIUhX8S8QZ58Qrz8PSdEUHT9IutYOVuVfSqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIo2UCPKHrxTR3or4KtwDl7jkvMDrJL05Qdk9VfoHeMd8bnVFREyyPW%2Fj5EWAP%2FpaxZPv5aIL4QWb%2Bo%2Fn%2BmLBAIM15u4fUaSsq1k%2BW6paTeBWVSPr9Evly%2BXOSN0vBW6pKpN053DaGLbS7q5vq0QNfHueo8pfjRDQ1yAr3ZyTr%2Ba2jGux5jev6V1QRbf2QSFZmu1xaptBgf5fZ7YY6vA53%2FSAZ2lbyc%2Fz%2BGpomGVStv%2F0uxgCcO19S0UUHHMyjdEcMHIlhwxHKGl1TT4SIInjgdfs%2BGKgECVzoJNqtcB0nnJlamU%2Bpg14iRVUWi0ZBbTLUGDH6DOY%2BLFdVcyBKpmSQxZL5UacE9MxmOM95VuEutEvkirxd8kunheLdawDmJ9SSJJ8htH59z5n1wv4XvOGXT2380Dz9dT5jgzQRajW50wSNve8%2BnAghN2TtJCuVTlFXyJ79bDK8UQEr0Pvc3rWrmhnIbE2wnyxOlYeLjTExkp%2F2Ww9nRyOq1Jquxqc2fFKq1orT4SM9HGNUQ5WHbF0P4DqBPRRtVx6EDtl7YSpUXRAdKiYv%2F633aiXsAjoSj0S5QnwCYq4IizP0RjQidDHaRUE7AQz9lGnuGIzoeU0nEgiuxzGmRfCXHn7Lbl%2F5NbUCsOBVtPHH%2BOk87YwmNjFxwY6pgHk8uVATZtBeB0WY16TMQJk%2BZxG5dEGh3j2bDwk2hJXPcoR6AWjRfGsSxWrAP6BnqvSu6RupUlayal1OZf%2B1uXBBBZMmVtzs7%2FZ2ngktA9IRv19LdZEKwtPEEy7vjL5jpyANAX7ok%2F74ZjDcIlWSyxwOZgFFkbeJEZ2ktff6Jzx89AS5y4s4VOwsH1vBOfq0%2BgdzloZqVzuJ2P5qxjNsTc1%2BO44srHK&X-Amz-Signature=4a3d0d5a4f5b3fb46f5fcc831e5082ac2ce2c6052c348395f819d0699a6560b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
