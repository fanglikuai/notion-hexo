---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VY25SN43%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBVHtzif%2BhUmdWjqTCav%2B3538vHp%2FY5GXQ2ah3QheubFAiBczjIasbc3%2Bn798%2F8VDOJrwcOohvIJC4kLQCRLBM95Kir%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIM52s2MY4W7QJqgyQxKtwDmDxB2cNd5fTH7jtQpZJTZtHzr2aQgiArdisJ2%2BERJpNhJDJZa5205PCCAawzqUnvBNT4NRj3VDdstuOo199ANtBiuMSOYWde5W%2BWkw5g%2FOv%2BKqGxAF%2B7ku7IAbzzcLYrEut6TUaIaLoyBzIP4D5d8j8laAD2Kgp23qWNjVxNGj2ufmfr5Xy6pasNx6lWgiq2Hnh6tZ%2BJktmYsfQDkvhS7wzYYYu%2B2r7qyDOYjzvSpraM1ESDL8tN%2BG61iA8j8a1gBU3iF1R%2FOnaqyBVprrt9XOJs74JA53HnYCm3WynmAX8YhjQL2vefsNXpG2xlSfVHy7Fp%2BR%2BfTbvfgm4MMQQfjYho4ZMoJK%2FvtT%2Buh8en1rTQxztXL8wh86lBDUN0t7j0ERpIInmANPsUMvf4fRfxH%2BvrIAD3TWCoNMv5AuOfcMkW5aUHZ8d%2FiM8e5v1bvJl63XpjHXe6DIG9gw%2BmKYEiZj5Z8HzYpTaUYiz3DxzklzU%2B0q7HPglnn6KKJ6WlqsY3xSAiTldF9sui2u6LnBv2w2hx42OK6N%2FX0gQd5MxJCkM6%2F7ZfsvQZJvBK6d9ifmnONA621f%2BEOh641uAHx81CLpiijD7V6yzhC%2F8N23YNvxKLTdsgwL8it9EZ91Iw5Im8xwY6pgFtaO2LNl0aBtle7XKBirG%2Fo6N5hM9P40zjiQalVCzncZCalYRLBEGOFKdEBaoGgp3PnqUEnOsxBtBcOYtH1ls2ky7Qnn%2BB%2F94KVxq6HcdS8fpknf%2BXcpf9huJjr3GohUmnZzC73VpUDVdg2Q5IxPXeEOWR%2BycGxsX6993oN02yFBqW0mUfLCotF6S3N5KZyRQ9Eef0fCSJ9fTL888mxWnSbmi4b8Xk&X-Amz-Signature=0eb7dc5b2e03142e60535c9abcefc2286bca41b4baf942551995175d66c6cc85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
