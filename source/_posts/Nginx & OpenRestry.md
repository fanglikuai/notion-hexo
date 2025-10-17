---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCQOOTKV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BQL3vvAtjlAQDdWEVTxfFKJ%2BeZ3%2FD2s1s4vA7I9CKKAiBttWJ2PPNd%2B3hMYMWKeH5NCnV88FdDhyDOcbDCPXbkPyqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjnATRKk94dUgn%2FRyKtwD%2F91HQK9JDIPOnTDAyuVRfl%2B%2BRhG0TFz3L%2FlaJfLEfSIuuz4ny6VFQnujCmehQkn2Evej%2FU7SxVf4yK1J4HPE7ZQ9e9%2B21zqS6ULIeW6hEA4CBpMUtxLZvG%2BrH6Zb7N5FzPqW8kwtwQsfGIfohi8YnRWddMiICZleTNl6V9eUt50AW7cC8MhxhajlwRLk%2B%2BgeX17FtbX1CVk3zvA%2BHwVMjNLFuJe%2FsopS03LEJt7t5SBUorbNI3SVeV7D99Zg9hagruZpbeu4cM7Psk8tm8I676L4ahli4o2kojJwQSShp%2Fv3o2%2FHrsA9JMKifB2eGg5EitEnHpgXFrZa1JEiC7Fevpv16raQS2%2FGcVwYX2pyxeZRlFuDBzixHgG1L166uO8K1Z3YINvSAWCXREOBOkekVtbBAE16he1lDvz0kZ%2BE47gt5OVy0QLHI58nQQy5dc9npbZCGM%2FPasLkmuMz8N46nChPnZKViNS6qBVMNwaufwv7n5CT%2B1bYX9lRN4uDbhwjayn4ytLzKtY5sW9hLBvAMe%2Blo5dO0RvGdyS3P60mzWPSXh4ZLp4rhRY7iLCOHmx%2FAGmkM3qTTg165lTIG1fM8uZKqpg%2BsNDscxIuQtHrf1aF8EHVoNiCXaO%2Bm%2FQwgv3IxwY6pgHXas08hAUq2zorG9AcXiPoSeYc3L6Ys1uPOQroFDxLi%2F0eSkhDKKCSTCAKuh3pnzVanlyH%2FRKfu6GvwgghOS8iCEEU%2BJHUi9b%2BfTNiBectboM5PBPc4HLCysot7rLr93i36Jol4gwe%2Bd%2Bd9WYfz14Flrd%2FpMJZbubLb4%2BFvjmAKLrDIFm9J%2B0%2F6TiQ6hnEioy8TDUun3g42XSk6CE8NlUVm7Tu1YWZ&X-Amz-Signature=1eb17505a2a5f5f56a1d3d555f40ce80563cd50f17e5bd3ebba148e5aa9a4612&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
