---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MILTOEL%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEO58LYt%2FFFzUydgFUx81mnp%2FSUBASGi9czkfuksfGIkAiB7ch2OgLZ5LOP5JpB%2BePAHBZq076EG4wTRKCuG1sqTtSqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP8goweDB5vTY95CNKtwDxkUjHvD5%2FtQ583qzGrTXDd9ZXHlQaR87CqIiU4wqmg0H8GxW9MALHHFp%2B4BarTU6A1ouJTMeTFgdUxa7B7z3Y5vjjteilra%2FZNZB%2BpzH8KvNuPi45Q%2FiT4Tcsvzch27lo5nW8FZGp0%2BlgX465x2Ox5%2FUAs9%2BplMGsCpkKlkg6%2FI8HBXKRLZPabVQQRoyWGSK%2FjKc3lXPih7KUFj0rox%2FtspcRdDh%2FLxodL7kFEDTo%2Fr%2B7sGUbzKMN0kLh2%2BQ8ZDQ%2BxcnHwLkFSL9g7ru39RvYESBYsA8GdmJkIO2Uxf5y%2BDAcM%2Bq8QbOfdKv7qbprAd%2F4LZTZ4nyRjy7iA47NIpljIrNjZ4RBRIa77dnP4S5TbC3PaCgz%2B7ow2irPL8BcKP8%2B9kofW1SWSW1AV9g0x6WpmJJADaXK%2B5SH1JIxPkN%2FCacsrDuIUCS4c8TtC42atrZs%2BoVi9p4bfeo189K6sRqZqTQo0R19wUZhKcRPL3vM%2B9gQ%2FKrUJAVjlUNT9W2sgQvMltnjB29JGJ6ZoMIKa0%2BLyNyYj7GSQAjAzFL7Wg3EBfBW0qvnYbpLU7Nc4KFVfV%2Bh42NyyUwX3W2fZyVfF4PfA9Xp1CS3WQHGZRwGXwD31wsuEc9Z3ncFcvDEdcwtYayyAY6pgEHwtEhXuRhmrzrHnvc%2Ba7dv0HwdCvBJ5B9uiUzWDV%2F2LaysggS3BfV9G%2FD6Hs8vKC6UVvdlp3ppnxQOJ%2B%2BHgZvmN5l7KDzTJ5sxslExzZQwggIqGEAp7nHty2xtBGF7v%2FzYpp1fIZm9HgHuKebQN1rVurS2hWg0%2BrD7Maca4dTH3HwVhbOrXui3qGfacsxnw3FqvDVqcjWMMTKfzQrNJVL06ibguiE&X-Amz-Signature=fbc2d0d8b04bd367bc8517b143537c6e1f7caec04ae0524791869c162c177809&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
