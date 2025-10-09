---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOHMJBTI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T010113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDtTEJPjtEfkfLTTvjDmxE7zrgArMcxur0U7jU4xoD%2BxgIhAIoGEF7e87gSrWzmaSbl%2FGJDPDxDScWZoglutRmHWyhNKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwlLtbu%2B1VQxMO0D08q3AOR4g1OJ1KyVN%2Fd6Dv%2BlwDixGLeRy37egNLvBi0Mo%2FqyJmlIvR9q5o8Epe2dN58OrcvbGRUlZmTeVNvyzyGMz6s7Kt8JYtOp2PbV2zJLAX0yamMGLaVNuGs2X3fa%2FUzFhUuJ%2BStCQ7GBN14Xr0ThC8cVfne7DJJxDJdc6w7LvDy3vZk8YcFUhKPSiUtp5qu8vbCojdMylejGOzk37A38jemG%2FsCepOil%2Ftm%2FjtZ45kXDfHpFuNClatQtk3k1NAGoDNJh6igfnufPu%2F93r1uh%2F8yqb6OyECMHxz5OmaZQg9zL2HcO2rZHWRzFxRlgOn6Rgot6Kk3Pn5XcSCM6hRnacFdZNsKokdwj3xIUVKBXZCOykJ51QJILcx8naFyzFRFt%2F%2F00lZ0hZJst%2FBW%2FriOC2SJoMJV9HWUNsk72DnjQeOswKTzwaykgBgQDCDBGeg0y5EJ1AG%2BENTxVn43NtEdzAOgpjUbMqKTbftXAn%2BEuU%2FKdtNww46Qm%2Fo4vD18GL2geCu0Lp6A4hYqxFObwLcQIKAf4XqFyQ1JBteNpQwV12ZrCIu6HUqweeWeevhTS98cYP3typDTq8gRLT5vrWtNsTakWHmMGMut0EQwYc6SqFsCO%2FOrl%2FU3i7gC7mem9DDMhpzHBjqkAa%2FR%2F19%2FKInWoXERzJ5mAkk9EDJR7tsnbtgMQOKTX4y%2FSqAEUz3gG%2FE%2BiZkn2663OleUdcNdnHV6C0%2BJ0MFIK%2B8xaNS35faBhaR8H%2FOvNPLqJRLAffZT6eDzPpgbh0uGzh1llxfB9phJpFt2FZydhijD2YbMXGVJHX5FPc%2BrvXzQYHASjlZOXXJWSm0Rz5vGPHMo31%2BGkdtpMGWsumUYlWxiOKd2&X-Amz-Signature=95597227a4be6d9de1a94f4a61aae69f141660ca480732f513a6e6808a545904&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
