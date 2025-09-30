---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTPUKETF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCICXVA%2B2ZSXbrUYz0u0rByrqljINFJ%2FZrUNdO74e6dj%2FBAiA2f%2FaNUJ0aVqPPw0RMJhuKPAIqdnd6qU32xP%2FAB4TwWCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHUIc5okV8%2BUsdjnLKtwDi4qTkWb6AwbxK805ibqUcttC4o9UgceMXKfwshrpWaPT%2FmN150p%2FnXGY92gTJETXJuLA65jRc70hRSk5M4sMdGdZ2WwdmNm8aq4QLlH%2BKF%2FH1feforNwcL4UjSZEGcjgSvbJ4o5oC05g8wXPK2%2FGcNEgIsQnfGpmskoxT1KL%2BzJmd7hfJ%2BmESP0aU1FooaoyjglDJ2y%2FWHsecaesw8vckQ7bP7WBc7G48wb1gpwjdOfkuID0r9w1AUY%2B0L3Kl%2FWlzOPI5WJyFl4kgKddFGTDDWSdNLjGERfqiIZFK02bAdQCLGfB3mA6vJF4V3w%2Fp6xN9HQtg%2FfD4eQTpL7u2o%2FauTqn25wp%2B4oZIK1pLr3fYVeSkgYiWzlKTLLb9SyfEwFWDUcz%2BD5prznWJJbEZSa52MDYhD2gieUaCcdIL3holnCV2IWkP7W69OGwrj3kWaJc8YD4h0fkkP2Op%2FWG0Si7KJYEVB8nRMyv37GIzCj9mXszN3ZwzPO4UB5EcdTuPV6zZJEDUCPVvPCPTE1EwA4zjVfDNxpizvZSOpV2JzH57xIUnoa3ppS%2FBV0D7e3%2FPncOKd4j0iOmqKwNnQTMzFgGzZtE03If52g%2FWIBXks9LIKXkQS0B%2BdPtgae3Aqwwt8jsxgY6pgF5ykxfuI9U1JehHWoqHsEzxlXUI4ooSsNGCLFQ1%2FMz6LR9KIlzqMzIVbqCRsPZqT%2F7jqahwX8bfD2%2BF6rTVBiQlBryjVmTD4GbHwFfpBVeC0SI7NNBarfWNyAQOvZMkbnMdAgAHWQVnssctPRPCWYL%2BwdfLY%2FUyvZmfV%2BN2Wlza4tZXpE2jrD5FVnfgil%2BDbMBfHBuHL7aw%2FV4dy%2B5J7i4J0L7kqy2&X-Amz-Signature=3e017a7ae187f40e4a07af3c90f6a99a9f558a75a62c1e9bab82f207ec3c6bea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
