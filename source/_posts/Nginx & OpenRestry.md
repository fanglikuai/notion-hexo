---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VHVTM33%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDCPtx%2BlD5vWqEBdU06vUuIF07iuZIqefR9eBK8pCJ2ZAiEArO4NzoeuN96oIDchm9HgV6%2FiDv8f18IMOOt3k5MeXGQqiAQIq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPuxRTtEnlGU7O5mKCrcA4VouSnBofQCgVwisVQpxF3TCfbKzT6u3AGmJthZRoOvcCuii%2BqHLoDWH4CtImL0kewSkNkOBy2we3IRdILoX729eCurjzVXsJNjdmSBxNmlT9Wwfa28JwfpXSY7BIHWuXVJikbC8deaEiMLDOwmwhMreopOQQR0zd6pAKSiYUs84z8r4BcwEf4N6kTAq5KCwkjkF347q%2BlOFs8L5MOhDbFdjdPXCjqR4rr7yIY%2FVk9sDv0V1CpiRyWZr7o1PMjnsvonyg9WUQYfLvnT6DOvgH%2Bf5deZ0Fkl0wzg3pIGdwkqyPvnswANtbIj%2FWKaeqGG6iNOyei5dnBYE36CGk5MXLwHrVU%2FG31GHe7TACocqd4iyGrs1JNX07Lq4i%2FmyucMIn%2FJ72hJl9V6cJVW2U1gbcW9cH9mkQhD94kXJTjEUT3TXWZ2puVNlY%2F3F2tAMOfLPnXP4Agae7wahN3YEQriJE2qOc8fHpLeVmJfW0yuO380DMLm%2BxjFgaYR%2FAHvAG3LDj8NMGE98luFisVlHO1JxqjvHeuotXgQf6ZturBm8PbLONaa3PkU7NH1%2F3IL5qoiVJSO9BCpsAC6bRhMZ5oZwvyZMyZB8H%2Bqp0PnXkOVT%2FoZsfLyvytU6cCemBNGMKnW68gGOqUB32IkhvC8iQDnQ9dS9G7vYXrC1r%2B8P8wc4nCXRwgf5GVOPn5YPlzitdjhUQEcxhfKsxywRz3zJT9LDlzMDb2U%2Fi0cYlKSrb4yQkJGBK%2BD2NA3j4ufQNTUpTbxlBAJNVEYtel8KZEFf5aUmR1gSZ8%2FqIKOdyksuSE%2BpghuIwBfA6OlV5YPmZ7e3WjkkqMAlayxitPa3GvZsbtBc1L1UzuP4CzQQNBQ&X-Amz-Signature=731156306643af71972d9b79e8f493f1d5be46a749ce4302a366e9cad162edf5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
