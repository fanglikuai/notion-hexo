---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUGPYKH5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJHMEUCIQDG8WWsSk9duX5dnQ5VNcgDdUA6L2etV8XuUO95xi7oFAIgdM95T4T4u1k%2Bpuh%2FfeomG5LPFby8Bp4RaKWwRReQ%2FoMqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFBxqoBIRHimpcqQPCrcA15svr5T%2FrbnBEMNKfjUYVGD4BIDqBsd%2F2tDu5jkEsuHsBJxS%2FI%2BZ3ULOwbCywTPbLu3w0E9IBMX%2B%2Bq4uQkdbLRgtJ0ZuCBNxKeRygk66Ya9ehxyHhdCgK16%2BiDZb7xcqT%2Bkdg8p9k2NojaPdV3FSHdTjdqGCc1ebr3SC7sk7A6bLsclwiivPAqoFmtwwSpDBmuxxr13MHFkKiaHyx3paM7WYolFs7Htp9qhCDgv19f5x9PmPW5m0rcMJPLWfD8hQUm9WL%2BvNgnU5dl1Adp0mYSM9UoKuCTRnO%2FhAUESZdBfXJNkWF9Ef7dNBv0I9slc0B08oxrUYRjO33n%2FdyXbZDCqTHYKm22qXNKeQG9j1LzQWCObraZ46QA2nTrMs2Zh56GTK7QxaC%2B8zvFDS0dh0krfSREsBi6%2BjdQbzOG9oA4ejqyKtIlpongq0jib5wQqiFyGkdDrQjpOrZHuWFcQ2iwAvBzymmC%2FhULInbDAnXaiEakvuvhC2ukhhWx%2BJsJD%2F%2B0Jx8ZTzrHKqzsOiUlM08Met7ULjSwQyx%2BZrHp1PtNe%2BQ1DpnvF84UqXEp2TJwZUi89oZZ4fWIUm7jBqPdRNLTGhqJat0GENxRVC8f9UKkjNfT1jTSA3dUI%2Ff4%2BMLScicgGOqUBEYProqdYR00LhVup8t8FmIWCkEgkulz8CHZvSYya4C8Kcpqd%2FiqwIxmwjhVMRAcmzeItOBFLj5oeTGKau%2FlNHWlyujiZ0u47A2WD7N6UtJPSW%2BDp3nMwBgORVSkYv%2FxI%2FdwKa0GApGy%2BafgwUwdC4zCEfh%2BftEIgydi%2FkLVJkjUoU7LlBzwV1vzgjIf83iRUEKguhiazrTkH%2BWC%2FNeM%2F4ZKrPRDk&X-Amz-Signature=2e2b9d62296ff0dc07b07d47c01f26f896d090776acc8c2b4d5a04412700fb05&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
