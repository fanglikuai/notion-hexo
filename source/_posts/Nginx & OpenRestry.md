---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646ZWLPAY%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE3DwxT4UbApUYJiEatmbp8%2FOgr%2Fad%2FIdtrie%2F9pYocyAiBJr%2FxA%2FCcGZkBVNzHI4KDIyrjRC4ep9SopHpYKEFBI4ir%2FAwh%2BEAAaDDYzNzQyMzE4MzgwNSIMp1yDxyukphkluEbTKtwDyJMbD%2BDEiGeTZWLXkaM2B91RmTQ8qOiMN660m8sZJMj2%2FgPQjvUS1Q9gthwujRWMTZ%2FOvqzPK9nrks8L2vz5TupvVZABXHrIdgrgr5oeVupUkyXceWoGmzyvHWrkgDQlLgU7GKXS%2BGB3nqA%2B%2Fo6899OR7J3e6DUsGRgyB2X9AQ%2BDZVZGmcdSKl4inVc%2B5ufgJ1%2Bmn7xvW2p0of8CUTyyZLUjXGGZsEZ2wVzHi%2Frc0i09wYL18cGjEwK8TNcAKNZGmwz1bpKeOZ0bwG2VVmgiHmT6C5j%2F1pHeQ01XpdfeFKFdJwwtQVAwpGfNLHKkyTZ55irv9nCGN4Rrmd%2BkBOjHy8DIxEX%2F79F8UiXlEFQAs1zyf5k5Pj1MfQ9BY0FymgkIpJikGMxBNuAVWPevS6XCBqmtUc9DQhBIhNWNaRWaoEU%2FiUVDcrAMCkxUaubOxs13ucBGNA6qtLNvSI81XVvZ6vbW%2BqreQGqlqLVN3ZPLQatqbD93t5PMPPdcmuAoaWrHZFEowF8AcULvjr8ylEAPrREmWNv96nAt%2BF7sOZOG6lgavbWH06zpUCSWmJH9RlR6v8SBb2J%2Fgc%2By1FlGeUta%2BBRLdGz3tb%2Fr1lWunzf5pStdjjp4YGIKwEOP6Oow9o%2FAxwY6pgHtpMIarkmOHX3OtCq3SydMgmqXVAmdfgwMLqqgjQKlu0BNztEEYTizZjp4MkiOvWej4LeynXNUGiDp%2Bp5VV5dENDFJ%2BjIYPMWG4eYb0vXcut%2BkEaHUjSvjmYRL5FM9AZuFamuSzs72Diy8BeVbin8PmbJUsZcwY7YRc22aV91aTykcmS6Jsq3xc3xASjzWL%2FogRTyShLgsQiLmNB50SFXGjYNgoTjK&X-Amz-Signature=f82064b18e99097c97bfbb4b228b8d9c16624cd3ef7eb8c68502fba7d53d7893&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
