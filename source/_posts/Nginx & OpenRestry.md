---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667CGFNKIE%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T190036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDq6dW0gDRvhtrfu416TJ4Nk56wKJUg0kNtyv90lISssAiA9Q07CYZFd7aj0cGV14mH4RXUBlHzASOG8fQ4uoDJIKSqIBAiL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMksGr2J3i08ljwK%2BEKtwDap6sxbvyl0Dinj57NrCvO6c8dzyav5U5Iz3Mfdc4BKiTFyWn6SVs%2BC4FPJqPAE5C4ayLzwVrb5SJJXcas052kuCeEnFZ2kAj2drI8VFWXBUADL3j0KOgFMIavxTz%2BWU4mcdIVt0vji%2FC0QP6rSoMZj1YgNTXbHGr4u8OgwlBwKnSk2j6wEiovPD0uw0gpLnKr24nBPOn8nJFffb1LbsLwniPdRmET6Jgz%2BiJRRJyaXEQ5YURon0z2qB5ufrJYqmYL7JPq4uq6oiSe751lKxHX9h9XLHON0NqAR9%2FYVt7Ok9hi4yqWx26DoM1khibnVdDiTxUVupUDpzt%2BqQ3RVIDjFVo3GtAcw9%2FWEbSKqRmUrFYQ%2Fz%2Bu1hLDXegiXwtoFgj2DBmPmvjAbypfYyCGvqzh2GwWAYBdUoTRzpKAeKNjFZKLRSovrmvAmgsO39NU1Bfy9lGAFA1bN7LKMG0b4nfaJh9dcrC2cnF2iQwVxnffvOcPDpdemwpuGDLRnSV3gumxCa34iVZpE0kYEB4xV1fEMoUq4jG%2FSXpIEVBHqlO5yZmwU9M2SZlCiYLjODCVZl31QwNFnuSi8zN6c7mHhlLmuz2oeQCYMR5WcXZUKHdPuyT4kUi%2BBeQpDEPiVcwpIidyQY6pgHk7dgr5yO0pDRjGLj18PjQkXS8Gi5B6qlPzb2d2z57yFDc32AueJZ7KIM6N5086O4AW4VKdzjORtMW1lyDQnUCxf9VhQlEi04uX%2FxmgarZoSEkpeB7GqxBKf51RW0u0eNUMwqKZ2DN9WlOeqTswnqoF%2Fq0mKslCVEwikNtWGVDvwJCPZMjuJ%2B4hvyp9SppDhNiz%2BmcI98794XAsfHksU4oy2QFCNZZ&X-Amz-Signature=51b00e3b8d669627c11c46f1041abcb6845739cbcaff9619e3caebcd5ff2252a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
