---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UKRRCH33%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDLq6uiaffAbGph6V3zmxj1brYn8zf%2FKDxhX%2BDDM7BsWAiBVO34inyldSd2Gw3gRxJjw%2FxGGpXgyzrbqqXc9w80Tpyr%2FAwg8EAAaDDYzNzQyMzE4MzgwNSIMltOq9ejjf7iJJpDkKtwDAMw50kBxjh38PBP05a%2BRe%2F1zrhBquQdfPg7ibNQ5AcoWVgwbmtLLwnLTt2sywAFO84FcNl9IPtK19fwB5wAqH7Sq4bjA4NCyRS%2FT0OqxC%2BEbmGi9VGZfXZ4qloPsmKWxXn%2FV0sX%2FMboZLwB%2BUGRTaIVNcqPAcNfWWwLOe455CafQaekIBeO6yK589PehLoQqrm9kcc4bfb3cvcUaxd5NW8rRbcDn%2Bo3NBaMtyyy75lCZUOEEjshU7H4OQRFRqODJbXSlcmJqUlK%2B78wx8x%2BlWq%2B9F2OmnckB8kK695UiY%2Fq%2BzII76kOJQAgJQdIHJnAaGqNLUJLdZnyw4SGYZAXk%2FFgwTvCOW6FfpQbGp460aVfDR7XImPHh4KEHUABsAbMC%2BJWOz%2FGF5KHecKXxIMxhYzqIRc1tAEHJ%2FUTzkLniq9Y1vX38a5LOxZ1buqt8oUVLOoDbliisb9Snh4pqCFIBGX%2BPED555EzPkB2%2BUcP%2BGZXNzYXLcZ%2FVYlyItcVZ5vp26n5tT%2B5B83617%2Fnljot3cp9LawmNChviXqt1JKboqRUHNmFp6aOH3foHmQAvrFFuNrxLtdUNgz7XPlW3t1C0C6oP7Chss%2FI68pR3x%2F2U9Luyl2kIiAs774fKwjgwgtWxxwY6pgEO0lud%2BqHBpKSVP%2FBRzFnzSPD7YOMn7cK42M4tEfbJkoRQGNKIclGb6JQ6q5vwHyHCN5X%2FbZdvkqC4bKmM5IyYFsPn0NZryTnX6j1ulhr4MGCSQHfODRwblEitWO5xiby%2BADgo9XRmOiDp9cNrz0azDbkU9sZyhYF5IMxBBKvYknKzVGfi6V5kkgEdsDFmZda8DRX4BP6qUHdxBTOm2ATCwTOXlE1I&X-Amz-Signature=db30678c744b452ced649a602f62681d8a936677e6ac92779a27c3c973770448&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
