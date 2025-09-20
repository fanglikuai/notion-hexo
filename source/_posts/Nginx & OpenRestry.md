---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEK7UCFM%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQD8JaTz%2Fj1soRzifOoIHTWQkB6Et6B4zW%2By8iEXOkRxKAIgRVs0PI1FuEcgE88iaJ13HP%2Fv7gG9sAeTrY1lvN%2BeX7gqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP26T1eDy%2FLsbNl3ICrcA8bSFuZC6MYe8f9Nwvj792Y%2BhPS%2FoO3O5g%2BP%2BS2%2FuZfubXPjWDXgGn%2F79kdzSNHHjGjP7HEHbgo4o%2BI0WTCxZdsmVCe7g0XFbYHYWeosRMGpyVoLvfm%2BGeP%2BP4cNrOGXoOoGqr%2FrNgfjzW6BmL9hx0edN1ubrhFSFmq1E%2F0lt3ReEupVFEllbleVyWKhi7tBny%2BCJSIfUG%2F6U0vnl9a94qyY8aT5z%2FZHeuNoDM1vH8Y2HPwFNH29FPkBeLiTsAdX6hTvEgc%2FU3RupOnWJOXLqpYOaN8xHnvZ34X8a8h5c8ZNojJVbPTf57L4DXfNDlU5mZTBBW5gUUPDuqAQr6RbqoI4X1Bq9mVF58Ek9qLgXfuOWv4AhQbEy7n6g7HCKNk78%2B%2BL5i%2B9QPr2zylz666XtRdTDFWhH6Bi%2F6CHcqP7L3afVLF7AyzCFJ2vuuEZIwBH%2BSJguVNuFLC3EwMrmd6BSRcExoGED%2BwzJpmrR%2FrqphGS8NukVSV%2BD1uQp3jOCK3KCSXfhPMB77KEDkUM5vWs8PQrb9fpnrUef%2BQAO%2FRj1QgxW3OA3JlLw%2Bw3voZp01EE%2FScqgcMFoPO33DsKgDLEC2LoujZ1Yfz%2F7QG160KuPSA%2B35s8IsU7L1jsRZsqMOn2u8YGOqUBpShpbIwnWJkqHrJFKNT%2FOrcN4S8l14FH3ztPFRpWDz9K%2BGsSDh0odpiOrrUhbTLjeijklLyNJy9pW8kCmZeMiTqkn6OgeWqU9YvesrZFaJP%2BTNNK%2FWykn42TnFEL5psfZ2WeQ0%2FaHQ79xSRwYr5%2Ftf8HFHdq%2BmOCzuTnmTD8vh8wPVxtPjNhb4FUSxvjKy2jmVZJLggUofEl0ZDlbVEtgJU7c3qC&X-Amz-Signature=d68225aaeca0c3dd64e64544562c085cfaa7a77afb610a1f3762ca97b43a4009&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
