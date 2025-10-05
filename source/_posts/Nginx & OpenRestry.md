---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624SGGBZQ%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHW2gMmLa2uyo7FbIHnzFfn1R9aAOiQENWilVTsDgr23AiBZik9JaHOkBnwWZDl9Txexy8vXuQqh%2F1rYq9tvtNkNJSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMuwcXRq6Whgs41m4GKtwDIRpRmYCmp%2FG7mFvB9AO38D3HDYmQ7f77l77zHdJ01HKYjbpNd93CsR9qeIOgBCyg1S%2Fe%2F1tlv2dqbWdWr%2F3JmOXT59gNfHTT%2BQ2%2FiyAH911VSOzSkg04yHOhWPg09lln0XaI9GPt%2B%2FeuMJUH8XEoHdQkcsp2ZxJD21oWIKfO2OMB0fqdin5%2FLw7N9Gyw7xTF46cP8gWlVcrAS2pFyxXxTfutUda2Ac9S8VznFOvWzNKSqPIYKZEEFbUNRFf2GL8OYdts6NWOBBdNUeHYUnDhS%2BFAYfOuVfj1wQNy2iLAdRIMoLrD0s9OJ00J9NXWj6jMvHTN13vvB8Tp17oVD4Pq7aw6HEoJ4Wt5PiIHBz8Tz%2B9qfA0bviwJt1rPmZx6o5yZx3ghDBc6ylRPcHEl87uJa0zjKS98kYr38iI8PY5TIXXXVldDgTnIxmTqZu7s%2BBnE0aibpVo4KkUjvbYU%2B1KjW2UfP9rT5F5vOpsZGfPg572rhjNfkr2kE3FzEM02QAKYxttD472gk6%2B3Sar3oankuhYJdXGji7gphcCdl0%2BfKGCkHIGZzfCyWM33HCrgwrXtgHivlM9ZbfmxFwgci6llZMIGkfXPcenYZHnRzF6%2BhHtnIM%2FIyPOdfzE02tIwheGGxwY6pgEPaWsT9v7tVYgouS7KoiPrgcx2oihcgqr0BDPaHaAYx0XZWjk3TvAbPFQ%2FGeMpJRiys5qVJTwspk325oHUsWXLT529koKxHawjK8jBVksRDSJa6y77vLkYWsLTqeR%2B4aGxufmdxsCB%2BAl2bSbmCwq0YUohZEocMohg5kxYgIJEyZD%2F1YR6Qy%2FNk43xMx9W2JcH3cpRhSr%2BNbhFVCOr3h5eyynr5dHK&X-Amz-Signature=a48454492a0a1afb56c985a8a97bab3252e83c1412721bb06b9b559be587562a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
