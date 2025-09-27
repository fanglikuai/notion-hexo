---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDVOOSOF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIGyrTb9ZFLfK6RcpVePvjnUeLw0iX9pWYYF%2BqvyrTqstAiA4ux8jihGVNLIFIgCbrjWA8IXR5CbmSShtG263HuhY9yqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrCjWV%2B15PVM1rBmCKtwDg%2F1P1Bf7p57mfaE9Dm2DQy9lMex8qS1rtoIHSuNgeI2YkgX4NoEjTNTBI1CqKr50bWwgotC04piRAsX1c58%2F7c3uNHUnotLgoCBOjXVoKi0orziQNvX3sJhaILjlGThD%2B1kGaPh8e7xok4nhp15F5kZ%2Bkf7aKbkBzZTIiwE6kPOezSX7qoCFjd2BQLNfDI%2B5PwwFPbX%2F%2Bu8FdvN9i33u5zuk8hNmWWgYScpd9gmpzF5tz3b%2FLRzyxLR5K18w6xd%2BCjFh%2FQ880bqlaP%2FNpvFU1Lr0dFj%2FjRrDL3bBc0%2F%2FPEatK%2F2jCSR1z3tJuLXIOyxfSvy0no3bpXjaFtKm92RMfFnvWCE6rdCpYXneZnLfuYaC%2BdI39FHujD%2BTDNLPWzQxFdo9OXOhHRMHbXrBci4anFY8in30B7NjRJft%2FzTueK2pPoHqJI04a7jtiBe%2FZNKI7E0UTPifY%2FkhNOtVs0vW5f3cN%2FQ%2BTAX65BSXZcdf%2Faff8iH0U0BC8JDpCilBX35RzH60cx8Nr%2Fh5R4Gdv1jSbb3cly44tV6VKKxovDi59vfcJzCFWyiLWaw7fMOIh5pt%2BPGy5IGgFqhEzPFEMegbZPdrs%2BsxFHz%2BVx7iZBBE5cCojHOiSA%2BdXBrwdCMwr6fgxgY6pgEg7cXJMZtmLxTT0cn9ox0l8HW6CNbSGbT1sIyDntHehz7OY6blQHEcbWBBmw%2FxkX4p5Sj3drEUurA5F4vYvzwpYD14foR5GkqMO0w%2FDnpYJGzPRT6LoXFW1NdMshuxMM94Y2Oxx1IleYP2h3zDiUY2aOr9EFioQdsWgPhMpOrejTlI575ZIN7xYPABH8hI4KSkEwi%2FF1aBpcHqfvU7v9SEub6qQPdy&X-Amz-Signature=e43feb02f32cf8914ca52878e89ef079cafbc7bf100ce5c55995a2ba3fbdf9d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
