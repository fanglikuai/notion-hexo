---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMKHPWAU%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHb4%2FAetjurL2CTUgK%2BUnRBIKzSELMkpJYgbD58vV%2FuWAiEAlroMO6hBA8v99zzXBwKhCgaVqJMcKvluFK%2BPKPeK5DQq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDH7fmrK3hDeEM4rrUyrcA%2BfS3RFKwn0X8oxDjQ%2BNi%2BuAsYJFhI0AHHwvmpeyTGzOJG0lafy45u2Xj5jPQRg0Qds6R589Qc61RvskYXa7xy122jFta4bDQGTgkivFWfV2tAGE9WfGbpPvgUJF9EA6457IHQJUaQgONuhUFvBOwnwHwQBZGq3eakqnc7EK%2Fm4oYKAPDOuBMHtCKYvZTxYNgfpLszIkIiVFpRCadN0pD%2F9%2FQBVFfW2bQcLYvwWckYZUmS%2Be1a62PqMOE2gv5D13tIjAb6D3hhv7npiPoHNRs4uTSGgmhtYunylJ54NZxr7DBOjXFy8tlWyOzJ5a%2FTI3RVLIWkoT5v42LazcZbseIjprHfesB0WuE4%2BzS03NATxpzLiKQy%2F6T8GD%2Bbhbz3SWtHs5d1DG750n8%2B0V9p3e2EZmbPmDLxIG0GkX0qIZk9gc2zQ3HkPKWMLlvb5H3lZkbeYqXmQAqyD2MvMzkthbdIR8gDn0s%2Bj0B5E5Tt5JcpCVNZ4hHU0w%2F%2B3RyXTDTj%2F7UcBEQnQ%2FHevD9zhi%2FaNBS7zKUUzg0TE9S6Q%2FleTjmdLz01BR8gJrRh0RQi%2BJRSWbZ0R5kfwQVzo6KVqgrN2ZAfETo3lSAM%2BWvqLH2fWrLIw4pt6zrze18M8Rkq6jMOX%2Fk8gGOqUBuP0L9oJAlWBHBQh2UORewMstMBoW5ut4qYH97RJvrMBVjnijBdYrwh1HAQkMBMXOIe6YE0wKg8tm8zbc9ZvQjePyFpKIsm6D1M0Vhfd24rKlf2Gf09YccUSMIC5%2BUY6mAB5HROCtz1CTUmaYzE0xBY%2BvX6%2BJT%2Fc5mj3AGpWkeNpnBxwHgYNBVhpwlBTV%2FW3wyzLb0HR6gfVpHv3IrWFlIo%2FCak8I&X-Amz-Signature=788023eee2b178fc967837673c2d9fff6591663456e18f9e6e61c0ddaa3db2fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
