---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMA3SS3L%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7V80cxeTePp3HhOi4PkBrQe9TN3QPex6P1MDHTLOFcAiEAh%2Bn4IJe8dyQQY9kxwzruoqc9Q4yfFh14jLChX0sFvFsq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDJ25Hu6TjU5%2FaYX3KCrcA5i12u%2Bm7XqF6PKvpyMG244I6RJistR7huAb2G6QDdEYawCtboi%2BQ6mf2I9RSvNTR3axKJa2vSR6FLNqCbYQb7tSfD2C5cybuOq2LDzBnnKmmXH1qRfUXEgimLXyKlho14ZulfHOg3dc4uDcZhYGJqrCRbTD9XATeRmfRCdGSzFIuCj7GLmKQZNmsXjlmz%2FZeXIH%2B4DOkaZwVPiBUanWDuiRPe%2F4a6KTfX%2F4CXrpoAGHq%2FueaMURNfcRIqTiok5Z%2FpCaIg5cgg2pBqmPrPezmdj%2Fm7mQt36f7NAiSZskEIa6ZO%2Fsjl8G2ego2F8fz1YYPFCcIt%2FZ0JmXd4tLNxvHO6KmNqDGzuuoHHSZkX21DzU4zUt8TwIHpGuea4Gv6jPm4afshdaE%2FFm9Wp0GGy8CrFJ11vGDVDvHAC8Bq9M8fewVwixab6DmX%2FaR2mMS1Np0Y2%2F9xlfS%2FD6Xz9%2FIcdLBoqxlX7lqJ%2FA%2BwM5Qe7HoZql2oNR5b%2BfTm0ai6S8atKnO5vzKBB00Y4FAgxAkanRIEiq6Cf8U1JiSnkKBzi0qRhPFfX2I5T%2BfXI6y8VVfz9tliaKsO7IS5ATJ8H4RCd2Py%2BvJeaD1pZ3XoD%2FKbiSuqh93jAUPypaTXQJKVmqUMPjv7McGOqUB7qXWW0osjEu9cslS5xU5FWLxB8wOI1r%2FkbAI3T3OWhQHSI0tu2%2FsoMIQa47FY0n1WsZbYYtAnEWfCkAlT4n0LoKORYflkXqzuUMFOGnFvxwxvfExF0tTkeA4UON3Vwmwj1JKXFK8nHk%2FylrV2fEfDLBk1y4Db1hX3wqf1ZW2scgLdW%2F9PkwpxR%2BsU6bPLj4dJLbUnVXy%2BQxIw4GlCzZPZ2%2F5wzlC&X-Amz-Signature=f395f4bf0a3ada45b27e4b13d793b22e20b304612b73efdfb9caa19d1c7a852c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
