---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQFFFJVV%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQDjFxSmBjL73KQULAr%2FhH1J6qPIDCSF%2BZAIVhP5icDGVgIhAIbNdSz3kpM9zoyWUKjayUua3ZKevOg3lXyPVtsWK9CGKv8DCCUQABoMNjM3NDIzMTgzODA1IgzeeohhzQNTC8VfE8Uq3ANx%2B21PGjWxHIDvBZ4gmvcrCQCFtSE50d2eXlhIS%2F403T8BbK1xxvb44Ao1soJbKC%2B5s2fe89L03rJH6vAAm9xe3hEhwIchrB%2FbtHtMd25wWFOwYf8T9oMdD7%2FSWqifkiYYvXn5AnNxBz6mg0NnLU905qBXN3m8nV3Kb26HNFtzjUb8lLNUFSAoCAixPfmIMkzIGghk2CtM%2FDGZlvdWVKf4HkBXxp6Gi6urxCH6D01e7Sdnp7rmwelbqSPXHV%2F3emGIji71dp2Qzj%2FsnWYYTyR9pcVc2yO%2Ff0yffvgIiyaCX5d%2Bl%2BULM60QXvwFlWgjv60Cn9vUpUCzBMKyoD2zvvL0wAjUjkzE%2Fu9d8d7OYR14awa2KQr4AIyZ0LBY6RuwdR%2FR60uq4sPSmeMXsvjvqWoINI%2FCmkHo8yA8czb%2FJp4GNLuM5IfnCj5afUuVNzDs7T9vUvCz5mKFAHhVeEJcSQIyNsmMoeHSt7kfHFV0PxAaWYxx11411VqTTVLCtHaMhYuE8g8pmicOFRnEvBfEWzF7PFIAbKtcA3XAuY8aYy9DFIa7NAWmMgD5s261sKGKmQAU3JbTmzgtqQ8nB5u45WLcFkYMR3SBjMwgyiy9l8r0NsmXIyL%2BLoqhWXSNpTCZzazHBjqkAeSZV1LJzDas1kdnG8QKMJZS9r%2FJXl88TLnyTixp4%2FmqE3ZeemwWaUGsx7JtisiipP2irBEFMy1dY0%2BC%2Fibnj8PuHgut88en%2FcIqNoIMBggtiFNb66ZpqIt0EDMQMG%2BUNMpbnQ4FYI0%2BmyDNf2u5vnIXBAR4n5HMr6htePmo%2BmVTAZpPvS4BmbBfmSGxXcrVSuGURvErO4f7kr0ITg2tCzgQMzqZ&X-Amz-Signature=9076727452a7e66db51baf6ae3cbb6a655fc51a143317415e50af563919f6b21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
