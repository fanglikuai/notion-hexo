---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666U3GXYHX%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCICHOh%2FymvsD9FVx1wL8wsEbJ%2FHXH%2BTGvuD8qn6I3G6vUAiBu%2BmtIFRDGggiSvC8nHRWDaMRtZs6mFfUA5R8gC2BFViqIBAic%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGJYXiHjuEmB3Mb04KtwDzQSANnLiPo%2BCNJcaQfkJma0Db73EWFsiO3fnPYnu%2B5bf7mW7SJxVqjPAXOsITyIGGyCL86p6BF385mGLVxUySgQYVzLyIaGLZRHUs3N%2B7lFKVF7OePUbcw1l7Vqq1Xssg9c2hLVQI4QwrdBBqpDIHkwGMiIbOjgV8WZn3g2cjL6KQvGa%2FYO0g7CHzHDO1S6q8aoaqKjgOs2MIaUpU0o%2FtgAYjgnPPQRhGG2h1LrlgfMAptQef3XkNGOpVpL1qaLrW7CoomGWRvTHQgXOgeDw0LxWXNUP81clJpsQFZZhCnDpwl8PG2loz82HrXl3j92w77KCTCdUmro%2FRv9%2Fcjofb7ww2flYLB4pKu9GMoUhedWyYemh3d3PwjqijNL36cIbIuBwUeiN8rTOtMgi7cXJ44MiIy1GbpBPBVz1qY5U8E43%2FUPIwC%2FrzCFQnlPRPSwLA2wsZXmiCfM37DNwCP1V%2BlAZE9Rw%2Fbvectt57g6aOuuUzlBxsPLdRpCQ6ePJVub%2BsiH12Aqrp%2FCTruriMedUTRuPtUGQiL%2BPE9c6VeKkCIBo2FCtCaPJkNTh7J9Qxbl4v7R%2FELAFspensxZCDX2lLwwvIVx%2BMZxRXdEZ4rq7Qe5FUWgATpIlt11rrb0wgdKoxgY6pgHFRw3SPoZ5W0bidnYYu8aEbZ2g3y3Vbcl5%2BuUxgPZxJbJTgVX7gm1g2FSnPVzwk3wPSG8yrFyRjM%2B39djr2jjchfiBbAOOwe1GJS5iNd5nuOl8TyxjVKGUX9HXZ0K2k0HOLLRYtWRyJ1LgCtDbJPWF6ZhS7wRdW0miieBpNYut8Kbf2e%2BHT9H9W2Ilpjg0AwEetGPTe68jY4F9xfj%2F47wM2Q2ChU0n&X-Amz-Signature=d769f35397b80cd97848da6c36f1e7a8d82bd1ca8b0e4654103bee3fca6c25aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
