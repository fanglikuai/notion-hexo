---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OUZWBMK%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICriY0rbtKxyZ1NQ29li6XiUPkqcmQ2PP0mtJAohNEQxAiEAgRy6IqPPr4zlgx0P1czxxXq1WjgytL%2FyMqqstxQyskUqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDW5xxxzCxz30Zt6GyrcA1LSulyZdw2feCyGtU2OD1wr6OhZFeLaXwTOeu4qaEB7%2FG%2Fxz0apfDDsd0we8xV1OhgKABnnbE2YVuTIrVeudvs6J%2BbM5XytMGynsY7wk8Eo6AXzgr%2B2uRvjUSIHGi9P0GBL8Cbtggi4zrHPgSKZgd6uYiYQes4z2Sc5MPqsF3c1bz1pd69LYDUO7f0rtULf6jGQ%2BQz2fPu9exiU%2F1DCkLGTfV0nKZZcM1AfUwodGZYXhTweU%2BJucNCsrG1d7gv2h1WENkzxAXUZI1PGfAbu9nqE9VPCApRhV9qitOFQPXu%2BdvzwYMBp1xWgKODoJftkKLNH%2F7Q2gDpeDElo6NTtUDDEodqkOFWxDMly7EW9kBUri0kG0%2FBv6NlptDP8QlY4HJNsdrKz2RmuKGpi1cxcP4pnSlE3%2B6aOb%2BpM8ZLNn%2BptwtOpG0tcxx%2BAqfkeDJVOET%2F3uaN%2B7YPGvvM0jcf7CjnWrRf9%2FCjqIidNy6OsSLYJgYjnJg0cUxTMECbo5ZE0wY1er9oDTad6ACGe8mqJRBWVKoRM7QEjBg93gJU1aDqXDb0N3VAjtwRtRcdiDhBaMbJ4OvrmpVTN7rKiJXRfIbmTG77wbDxs9VsLhL7n4e%2FLPxGNCcKL8V3k0BhtMITC48gGOqUB42mEvwolD7e0owwqmnFVLyskrFroAEqWCYogKteuQuRIADSi9nGoVc9%2BQZpNLCRQhkCPUU5eZ69UoPctbaCk6%2FfNYBz4CJSciSzlZg%2B%2FW8wNZxp%2BlQx4MUlzlQ6kkAQQsUbu5bH%2BcAuw9F0lc4wlzZcXvBca%2BuA5wDGjqoqTWbkxtFscrjTcI2scFUEvJ1AXVNjwU%2BPiGRr7RQAa3R9IQoFV7zzU&X-Amz-Signature=e66afa06b404bf986bc0a3cd655365856bc77867ba9aab8f71ba3cb7b8d62bde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
