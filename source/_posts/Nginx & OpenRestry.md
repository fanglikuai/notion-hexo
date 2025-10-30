---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPDSA7JO%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJIMEYCIQD643%2B5fQQ%2FZoJpPXRUI9%2BQ69RFyD4iXCDecFBnwOgDOQIhALS5s%2B6GF7dx3KM%2BMNO2mT%2BuoRSCGwjOptLe%2F%2BJilEocKogECOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2B6lq76TANv%2Fkz0Moq3AP4e5L3woeMIlO7wVozVB4HMrgFbZyje3XHkpQqRrpXtMlK%2FbM8JiP1ACUuOFe%2BhqOu%2BhS78xSHA8vtj%2Fb3zEykNhWf4e8UN2opFD1z3jBLz6uXIDUZtnubWvjXQjKsZ%2Fqxm9vWFBydAcS7b5iO44OqSfg6oTmWQrBkp2i7yngLBQDEL7njfl0s96%2FLhkjb18rxgVa%2Bg42Yz17emrlx3Q9yKXU4wxrGMr8P7Qw2U%2F4Xj1vsCIlsjMaCKzhyhJPkvblMspyBWwCNq6hih%2B1VUbvsyICVjAyN1bTQPFkOgbFdS0ALnr%2FrbpRCErTIGNHd0BPPN9cNT%2BqaCLyUXp7Merc74GUFJitSi0EM%2BW9WC6mf%2Bq1%2BWfP4%2BERxlsB%2FqwnxqPfuVQeteHxjKi6dNvwg17lz9WfAKnXX5lCsfIRX4WEReXSpH%2BqIcSnNSNkDrINrOSwBKwSZY0dU3qC29lGJmgkaTq%2FMYjRiFTTUzYlI1VolBdow0i1IS%2B%2F%2FxTxbxlJ545Ikgf59QhiGgiuXaqyjTT%2FwsUNYehCLmlpXbnPC2ZmYPyS7QJsWUCut0gaYjpZpiByBlvkjQx6x0iIZdx83lVU1q60FXTe1dwB%2Fk3y5GAepirFLG9dETD3HPoF9kDDslIzIBjqkAVSjSXEH%2Bua3g7YvAbr2POoeJ6OOYk2SHhQqpkGoXrZBj7lMTsWTj%2FX3uOdl93U89G4VPxkEnUF04lMWeePFuSzmTeRG6r%2BKXS1lThT%2F2xhs1C%2BwTQQ7IGz5UzOu9HnLQwurKO3RdBP3jKXQhDuQ%2F9Xdg2EEEB7V3wONnnsQtGMjowGiiF7sXO0cAwIlQ857a290CnUL%2FJVMVrWCo9StfmgL%2BYOp&X-Amz-Signature=2bcf0b297b7e70e21b5c0dbbb6c1bc637a5ae8c0fc977ddbd019609df7630dd6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
