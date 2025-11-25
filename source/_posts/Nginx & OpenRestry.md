---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XSI65AOO%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3zoBsk%2B1dOjnog%2Fa%2FH7Hdc6%2BJf%2FAEhtXM7xrKsIf%2FjAIgM%2Blr63mI387oggPq8Kj%2FFPz1xY2mlw0xOd7wQICly7cq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDGeWAsAX2VGfV7v1AyrcA3IlOpIODm7WeuxiXWTM4P8RviQOu57nHHWZb5IlGs0L1ESluYeaOgAuWmW9Pg0zDG8CJrCaM%2B1bxhRiuLzLgz3n7ocizJbqPMAYUBwvuAGS7%2B2HVC3fsO%2FS8QAmK1RIUp279fGbxXVD3rNFsIEWksf1Acx6Kivg2qS6txXOepdGaCvV87yyRiIDDsrjGeir8KOejF9i2uvFkXgsgFgJtGjlFSBwQAqlpbjqvz7aEuQ8yfhHfcNDvcg8Zd8ykctLri1vNrs6gFfWKE0Fl%2BnU9rfR0%2B3s8%2FZd8dhhWyoCjPdEteToZGLA276PrguUCUYeeTSZAJSrpssaJt2arQDXgas1jlJsPb63sb7c9UnAiCHanEXA64k5wgPeDPtqMWP2wemnfAbQSDU9crOe7uiRuPRCM4cPOeAhJ0uJpDa6kievwxJWiXhsJ0L1qxEpk7ANlkN0xnK4%2Fse05bruDwsYs0dx2Z2BslUaYEaXDDijhM4DBDnpOQGorU%2FnmzT4Q%2FYDBrRZjZtv4MAR5z0efQAqyJzgCe6ocTYSViay7SpTJmU2IkfYprg9KcmVtEaSNcctxWTTc8fpXPwVBGY2exkaZ9W0rHzKeiD8SqpECLfEXw%2FfhXX2%2FFYf48f0wCmNMNSel8kGOqUB%2F7YybjekT%2FEXtVAPqoEol3U1SGFzHI4oYJFX00e%2FSCAwHWmf7atyh%2FBwnXDS8vO9H5A6sp%2Bek4fFGoJz9xfASLrtgIgae1gULUA4%2BPLRS6p2KiYwc%2FdXscDfF6iFW5N9nYXUwVMNnwH0i0hMrV9AroZXfzLBgg3bUZj29jm%2FZDE73EF%2BfhN7uAGiJOFNmEIovo2Ph6g9ygOwtxERYtHc%2F7wxng5j&X-Amz-Signature=7c49f3ec45bd90e1885e0a42c7a3776256bd46abf5b3fd2405e843a303bce6b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
