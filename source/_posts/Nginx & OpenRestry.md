---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Y7UW275%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCXcnotzGQXwFAFoYtvOOXVJhZ8cUJF%2F2EjSERtlibGYQIhAM7CRf2QIEeXoYRVaBXloRqi2Ft0%2FK4T%2BWAwE7xfnelMKogECIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyp8pJ6mMjYzUV4Dcwq3AMXz0KCknweW4tVKu2ZJOPRvlp4yMHsso4YZcXBC4h%2FJpdR5dlcAxg3MhilEKgs2g6AHGS9HS42lU2jrSDit3UVZKHONmCvA%2B1wF8ik1dv2DK2qcb8jyhXVCHtBYhHUyEKNnnTl8iWENHrWW2nj5vHdyff26Mt472Wwq%2Fi%2F7plL%2FYxIoL6c4zEmgT3%2F%2FL9pAa1eOY%2FqGW%2BfZQv%2BG8x4PrWpJw%2FWRat0QCTtTRIMaMxh3y46q%2FqL17rU%2BrQp4JhK77MUj3tO7fC2F755%2F2NTJB2Dh7yQ8hWstVIPj1Rq57phjvK2xIF6OLTP4KvXVJuFzmqYFSLEoP%2FaBZl7AssUCyxblDAa3CCTxJjFxKWdmuXcL5e07g1MzDn7JGEh9DPH45CACFkl9MOjDk66Zh9M8j%2FlNSdgtTQZF0%2BbgP387iX4vsZOYPE6DbMVXGGF8b042Oa6xKOI6PmKr7VTISHYjJbjvnV%2BKKJp0TKmmJEHkzufs4yM4JaJrpYL0afuo%2BKrnvjGRSPWSakdVjC%2BO4zCuufZupO%2Bmc95yq%2BCFXW978m43wW6VYpF337F2L%2BDIxuaPt7sd1bQ%2FGSw2jq1L4bhZOCgN4QELYZV7YakYNdroETP6xGEepzfP7S6xtPE%2BzDwzuTIBjqkASvdmKNtE9dKgKEKmJwOhl30tT2pKA87LKfIqnlWXhA2sfZoPet9POsTS2aC6QJolmrMKX4wSkkyaBNYJtFi1Yfi8m%2ByD6MqtwyHB7qzvkCh%2FeuJ7Y95p0dpArss1cm%2BeqhpESPnCxcGyG%2BQUbKlmaofCopXFxJtiJ0%2F%2FPq%2BjhoAA2kozc93dpp%2FB7k79jeeQWIpd5%2FKzSLuNiKsohGmRSuu2wCY&X-Amz-Signature=54cbe6834db34aef644cba78722b2f918f8b659ba5109e898ece730eea185010&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
