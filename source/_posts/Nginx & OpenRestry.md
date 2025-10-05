---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIGX7ALK%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD8eH8SGZiXcwy%2FM0h4wyJgSBwPmi3IqcAZUenYW3EGdgIhAJg03b1%2BEuSkJl7kpMbXyAn3FKkMGRpwnvCjkh9Io%2Fl%2BKv8DCG4QABoMNjM3NDIzMTgzODA1IgwfvJvwQ0TRT%2FHmgFQq3AOCjbCQjC8hhjCazYqL4%2B1bIM3Cua%2FIx8wBn66GXnSbNRCqizSj6KASQtfpA0UodKlDp99kMY8tHcH44rVAA6aQcG17diVRep0d6Ev5qzD5Xrs8pGyjaX5jwDT2HNVus0w7MVMMmX68n8QsfHguPFid9RWqGDp9aYqYLDyXVtdYtwXWO85RqAISK%2BVVFKZLFM1jvT5D3TukLUMBXbQOvnWnWRnHzUfaJELLtV7wAriRhXjKuPA8Uy4P7vSRigHjHL8%2BM7H%2F9%2Bdg6v3QFtXcxjb1lfG41VfXM79lE1sugGnq5Rv4FlSa8jBVziujsviIaXFyPGOVVDXURny8ENGFMcTNkoyaTKDSB6QK2nTMo3DneVKEy9gLjs0cAwBNZ6Ek16we6fZI2JpIr1lz1VyZWwodvV5ZUXorsG9X%2FtqaMyX2eHYVt6E0%2B1RZwbK9CRYwmj8JJpvnUgxp4YhqetkZgZ4CZG76IU2xh8HtsqY598mJIi6KiupoN3dtu58dvrmzogtfTxoLOgpuQBItbxNd9A0lHcV2PBVFlEDhO9cS4Du26Dnzwe9%2FcV7dDYZF0GFwjwLOdvdsr7QeJVTHxE%2FOa7eM6GffVKBtCM2dNsPJXjto0dvzRoux%2FzH4EIA2tDDg%2FIfHBjqkAa8NR7YPFxPG6tDuUhKXYhZY%2FRnd%2B%2FJ4ag2fwht2KfQzAa5Cz6QQrrzC%2FxZ0vsE4gWKw2L0homItlmI4T0a1BtoW9r0lHIH7Ic98Nuz%2B8gjrKjHAA9LGCXlkhCJ4Z1e7n8L0ecZwTW8gqGQJ3nbBa%2FQkAOwK90yfoSf21bHRv6dBbAcp3UvyCNiDY9YcUxRC5Vo5ypCENVd1rnsLso3ue%2BxtCDla&X-Amz-Signature=8f51dfd118b362d9112f543537dc5a57ab4aebc0698b38dcf57be653a3072bee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
