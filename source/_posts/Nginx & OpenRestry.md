---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVO2XU6I%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCIDEuLqvVersfKSjWj1vOR%2BZgaxiq2YglH3jc5AWWb2EkAiAbjPEdpPQMWntw6HkZwlflP9UXCBatPgwK1V%2FDIuEf8Cr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIM6r8xUOEW1Cn28ZLrKtwD9gBp6P41DXUZl0xb%2FMytkP5vCtB%2B0RtIVWs0GhtgGSqiRrdjphQhe%2FkIxgJDM5M7POMOJgOMN61qps5xN9Nr5dLqkQ50zvroN44U5eZk9P8fAOP8GsC2sc78onr99c%2B7rPat9ww2uU4NBYu3FFDgFoGJoq2yprukckvqsyD7YHmxvJdaK5HNmVDDLDZ9xLV4%2BVxjf5Avk4NV9q647RhYtuNjvd2XQfIftkSAMUJsCWjKAIkCGyTrbXfpRLuRWqc4A9J1YooJ%2BSVD24Qex4Q1OuCPITkmig9gRVfaYLL68ZHyo9RCLVIggDRcce6VjOd8GBXbPXGcy0wA7%2FEBOLEDWjcshYArr79ImaBfydDnEMZlL0X7Ncr5gRgRZWHa740iFoYPpTPYfXuA1cXbQ2VXE31zW2bcip2zNFsmU5CUyqll%2FCt1MdX1NM6sHu4qRhTnG755FDODQP0BGKBGAvI1d644xwB6BiwIox9gpHvd7NKAxPzfa8ukBDoZtD3dG3fhF87qwe9ih8RDRqtMp0CCVmHACx4jrzoKAHQljClUmDvUb1qtmh2yFoXgaQLi8GBvEVY4RXOJyzbEeb2zcjTHpTcQMPSxA92Vp%2BDhRaYQ9MwKNZrUqPreAjN%2BmbMw67bMyAY6pgFQInW3U%2FdyIPvzwnaijOSaGzofOALIIo5fB9CpNOCVjL05sns0nJgX%2FpWVMWDvlsZB%2FQZ%2Bto1bxCnhUPv5T4lqSTm7ebc2XBTaEVP22AvOHpahvjm93rLuHP0Bvi7vQ6zZ8kZRPN1ixXiKjyzHVLnfPNSM9Vtxh89Wx2ueMSwLnor6E%2BLrq1M6ZfJSD3C%2BnkGM5k9f7QUVRzmH57WN0Rgg0IiWqyyD&X-Amz-Signature=293422967596a6b8b21624afd8d269c8b34ebae13177130e524be8325c6f1b30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
