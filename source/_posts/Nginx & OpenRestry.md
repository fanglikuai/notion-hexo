---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667K75WGMX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG4aCXVzLXdlc3QtMiJGMEQCIG6UZ7H%2FT9QekSQjY6rg%2FFQ%2FlBAnZaOgbLykqltit2jGAiBB2kZFDPdIlIOGZCkC309KB0RuuIcsclZGPVVojSqt4iqIBAjn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FfXJ6uorrSpyKhysKtwDF4IX16dhcK9RK9ce56QyXSlzPj58mzIPQFIEk8RW5gjIbvqIZOsajLTar%2BdMfc9Trno2Z6%2F1NzOEa8v88dG1JfQJzwR%2BxyXm2paoBOGvU1YxvZSV8G6eYiLoMa5%2FC%2B2uzt%2BLVrTScljvpT%2BJunNJhBieaSDOFTIouk63On4mTiwHA81JB%2F6%2F15d8qvvL5l6YCkwbFTWFSkiW2U5SC8b07TAlNv70US3TFbR3z%2BYbSnywyZIv4bX8h%2FNeD3YjLuUEoWZkhQNR%2FFY1s%2BPVzbjiZE7tqvIaNCm0NzrFjK86cauzdRnzR%2B%2BegX9EQFblNd6c8Rve%2BKjBnrMTZfpoKYrkomhwLl8dArckvEg%2FsGlQn3iLw8Cwyno0eykH1ADKDh0fRy5Gqv7HYxvklNyLFxFMoRZYoJrVsHGzWic8hu8E%2BrPS%2FpqtYIocRZOkoIEvkdJNFlUHedwsm%2FKJnKTFioufvgrQwS9aIaw%2FH3gFs2JdBIsetTdh0tIJq2ORydSTSb1VMqpx0PARlKIpea77ab%2FCEj2oJTIX98gHb%2FbaexfOJ5ykgc0wimJgSL0lm4pYkTXuP36YypiHBOjbw9H%2FJ8F%2FS7I5qJSw3eVj6uSmkc2QZ5a8th82FBB%2Bj2mhM1owtIa5xgY6pgGKoFpYdBonDOvUcUeWGih2LWF4N7lzkAqHyquaZJYI9CpuNDtyoC%2Fh8DoUGSq2V00dzv2Wfe14TyeZNKmG1NViLS8MhnzOCxY6DNaxHg%2F4wMZVjX%2FJlbusQgp83s%2FwccDBA1j907Va7MDQ%2Bowa69Bq%2BeXzw1x81GTXmGYZsCC176Bk7s5vT1eBp0%2BXN%2BCrMff%2B8nWeukK5cNEm6jTtJoxMGIMC0Svi&X-Amz-Signature=832d092f175ef1dab6c6db9697e8381f783b6b4d813c9b4f9ad658dc3e53ed4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
