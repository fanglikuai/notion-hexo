---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHFLRMSN%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJGMEQCICrJ%2BTS7vsk1QUiJRkvDmvxCN2VjueIsINRiBW%2Fk7O1xAiBBsFlyy3K4oYMHobZ0ClzPBhyUy8VkMJDpRZezU1JC%2FyqIBAjj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMzS%2B5vSxi9sPUHMpKtwDu8pj2EBvj02APmCyWQTYKLO4Rp7wavPcqirLX4Mb4OGvUeGpBEXdc2DM3091UVWBiI4jf3gzGT53%2BpWDYjieU5hPM8TXOUWLd0QiHMi1mLXRo5OxVjRvAoLZvPt7AsHmOPj4sqQdyvQO7zZpDndsa7ZDCt%2BB8VMjPjoPTKnJPMTcQvo9M2wZuWSJtdRDGPo5vEJJA12cXUdLqBTQulsqDMyohu61XG%2BhJSicztICepIpyB2MNGTPkoeIcjo60JWF%2FPAw4LA6WMgND9m8EyKrzhGOtIroIrCkxlZKVuOhXG2%2FuWVSMTqVQFa3MWaY8HwrwiVIwEJixzzYdUcUt2ZlQH7svDnPBKprH0KUgJn8C%2FBgDH7seyf7Zk%2B0iMAhgUlOZ1ZZQlDuBcccMxLmbQ5kEUo%2BqWlBFEZ9gJhoj7YZNzhpQrgRQEgXksVZvD3h%2FClzfVasqy%2BnK6%2BUeTvOyeLiQxHtYgQsmrSRqGXTEYK%2Fsz7hIvKREIFVaDsr6m%2FTlIbFzjHtgFodHmdrzr5X5ElkywDCBJEHE4Vx351c4x2uA4tBT7ePLDTDxbDTynx2N6tfx%2BJLfdDp22Q07DyJtcmIhWaGxbGfPQxHoCGNOXE%2BQ2ICkEhUpqBuo71MEXYwxoH4yAY6pgFW9DGVxlJY9rT93UrwCMH3LCtntbMwkhlzdvSDwaJ%2BQowJnd7in1tIQTW36CnhDVUMjZe4iGMxpl%2FkrOmmzbNL%2B%2F8ORRPpL1l3OANsXrXAgnN8ACNySk%2Fa0rfXKUW%2BaWGuMs2Eh2E5Cm1Np7mjxVEgWbBubEbb%2B2%2FhcryIEYd49tijkY9c1I5eLRjmWGNvMs%2FzqexcByx%2Bmtog%2B0i8ZzcXg2ihBv%2Bc&X-Amz-Signature=50882f3e6c5b5ecd15c2159213e949464a16b8f2f6f55c06d6a4396e5c667cd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
