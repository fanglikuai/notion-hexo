---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQXOBBEP%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEbzzkzRmZ6B7JBZNRYFZvpgYAIG%2FMVWfpLGGgO0V7wfAiAKoLb7FSQBmGH54CWCWLXHerdMKoZUJjjBv9tFD5ejVSr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMWtjThEPcLznwdC0JKtwDB8VmQ0AO%2F3NkHId4aEDii0aYKXuqMLcpbmigduWwk5SeK2Yd22zNOUbQV9FvlgVPnzU8l%2BpaaKjcLo3L5J4vNDCB6Qrv5Gm0j0FYqfUbKd3vHlNH3H5QRd3Vh7DrjLLzeA0LPMJwy8CJeIYtfx1WSiWOkwpyvvbRkoWLXt27q9F7hJKIIEk8rWhKaLHhVtvzgVQRrfPoU0KSQWLFlYZD33VcqnnWjQ9Lg%2BJaW9tVzLZsr2cZNjP8WCc72SXyjytruLu0wVkbzAPHeeBOIDa4mfSl4FrR6W%2B%2BIT0iWDWkv8Ug2zBye5fxKsW%2BsL8dW%2F5XP%2BuH%2FPwXWbbUK%2FUOeD6yFi7G9B%2BHCV7cOf%2FlhKHCxbbjbEaiH06SV31PQNzfTWi6Mcdpiv16nAE%2BBhrEPo%2BKVxDpFgw1gqQaKh7vZZ0xVWotdyot3WMnhbG8rTUr33QKUiG1HsTfG2UC1EUdKyj0%2BtXSTIXhzXbTRSNM%2B9Q%2FSBna8RjGbEASbSFPNafVQXf1b6veyib6AbA6UODUR%2BX43qT4poEXlVu6NSKto43zg3fWi7SOPGThmonmhd%2FlSmv6Z5%2FX%2F5scKUDoPTFw9HkQseUuUElnG4ja%2FhLEaR1R%2B3SQCy%2FAoNX5Vz1gkXMw4JHeyAY6pgFGXoLBEjNt%2BmThY%2BzTW3YRB1hvBUL4A%2F6iJEL5AifBKEkOIASTOhas0uQXJ3Rz8pGumVE8eRjahSxf4NJweuiL5OD6BwR6JGI%2FWg7ytWq8ARxLjN8RwKSCTK2Gio75iNHi5D6OZwq%2FyKjZHASSxZinwNDJcTMz5Gh0ytNSYgm1Umo2vuw%2Bo85Ju0QUtyeHVrqHryIp%2Bh318ulVNEqPq77HJYty6rzJ&X-Amz-Signature=b526610c690f1d83fffd9481a2c0d2fd4ac4c58c4a54b7f1150092ea4f27154e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
