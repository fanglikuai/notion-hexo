---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2Q3E57D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWn2mcYNNYeF8HPJBvgpcCgcMBGTz4BFo6Qha8uSJrLAIgAYCJBMz03vDjkPN8H9rR6foXOU%2BbgyvPibYPO5z3j%2Fcq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMTti5Q6IoZNnxTy9CrcA%2Fxk2PsSqqYkgk1gilV71JSiLAAoeN2i9UcJk7XT81z5ZjbsOAXfWEwYkK2ZLTyM%2F14irw10IzOVQIJSPG8g45zxfkewDQ2wHzKkWIIRnXVWZWqldL%2FduisOj8zOCrBsr4Q8d6erCubYacUJnFE77ujMgjyhwRD1lKYLjhrDOnpiU8rYnf0i4NZTMI0v37L1V9JLYVi6oYf%2F7PE%2B%2Fc5L4ODsyThs%2Fyw%2BzXZun8mq7htkauNAZKGPJTwDBP2l7wGfrO1y6b7SAk%2F5toONUbh031%2F9cCwtCmM7kAqLSMqmkRhF6CLVhHN%2BboZZn5%2FHykTzTFmwgMSTZbupF%2F39YKBlvYW3NSU36hWtHFdMFrpXNJvAHMMz4CQVjDtWnhA4u%2Bkgr2S%2Fsn3ryi4nDXmTGk22GmWT1FOQg9tkWnneaYIflwUEk7m0MUYgs3DPPmZ4mRlj0pYUr%2Fe0M6gI5rTKyf50dhXHdE1%2B%2F%2FM%2FfO5LKXvnLJLXZgedZAYeKe7chfjBvwvMaQ8etstFKlXCE0jF7kKL2J4eyskKOUBFtPqR1xS7MbbUig%2B7MXdCKQP1sGFSg7xf5PImEMZGKJ5et%2F4GKhlyXAToLo57ocyA6DNhatD7x5Fn%2BsZ7QPO07FXF74SyMOyvp8gGOqUBGAQ09aZoRVgja%2FAeP4YdmMRJmlwysm1ntCyXATrAqxzzlvFEkzqu%2FleqCyOJI7M4Ur2Mwka9yXo9Gy2WZiE4hRROc9jTIXKiRU35T2TqALCtQJNO4%2Bhg1qQr9nwE5uk6DCiVIeMVlN%2FSLBrwrupJhMGADYpGP4bSoXKxrBXq0sHfdo3DlLkQoh%2FhHMiDHengr%2F1dKAppEgzWmxdvYV%2BY9MGjHEA4&X-Amz-Signature=63ff5119ef5a2c862ad1ce1884945b8c6d14afcb35bbfcfed10bf1f9cb18de03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
