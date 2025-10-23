---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UFVQNHS%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2F659yUFvdJyjXsXj3wnO0WMkaf2rrRLS7j32bKUxGqgIhAJiEw3u4fCsnPTQStyYupIxrOyVxk%2BXrx6wsOXGl3X20Kv8DCEgQABoMNjM3NDIzMTgzODA1IgxLzSwJSIYvz39sjukq3AM3hG3WcnK8v9Ib%2FImsOiGVANPdhI%2B6ecF6AqaUS1vAYMAzjf%2Bnu7E%2F%2FnskrJd4gCDzUhmeY8vCHOHYVITVtAKja%2Fq%2FpYLsP2pccrYwj6xj%2F55YmSUpIaa42wAMMnOW3lsjYT399N64FZ9oaaxULbbEZ2rcH6rj7UjZGRbl4YgWlhczQgN8I5gYAYqckyWAdccts18GJybyRSCZcoYWctJo3i5S4VR%2BSMdpCDLVXSYbJP8oWVykZh8C%2Bu1NkQgjYgXzjxyxwHdbBq8Y%2Bm2uqB68dSWeFD5BCtFSDNrahYV6isvtb%2BGXxIue898gFDXj2ZsujfOeU0QEQVknpShNzTNK0hlOmi5ERgy0aPMRIQNG9is45nloJ%2BuOmmXmkOIgvkWcyuV1Y2SaSmm7p6qfKJP0op4mogIXO4aBQDZE99WAMmKA0sNnqRDpIYoRPTBZQuL5h3gOMd0Kur4XUH44TDv3QKRCpGtppbbEgUkdNhgubvL7Hun4CTF%2FAtryQGK3BQsDQB2DKQchjouvoBfuDUyjpySaEwkjXNMML2bK3%2FVo3Nt2QhoYuvjH%2Ficeq1ud7riLATMZf1KWzKKdWmQDMd5BOfsjx%2F8PCgEnFe5Tid%2F7uNeDzZDAgzSDSYb3QDDs%2BujHBjqkAUezjWVoG58R%2FHxhnnk6vXd4Mn2X8EGBTB3LWzl3KZky4tio%2FiDiLLTIKKAWB3krs590M4JRt31kQTsp6n4FwB7VT1ZwpfYPU3pGPTHDcNwtsGscHlkF%2FuGMsD3fkK48CRN3NHm00ZFJ3qiYQ%2F3NYEbZoX9b%2FIwXK6XqUF19cKyqMvgErC9ll78xnKMd%2BGJbc2%2F8sWLSLdF8OnU9%2BaQjwXbAdueo&X-Amz-Signature=5c58656e1dc4d556e9103e1283a92a12beeef84fdd6e755f64bce7c03c29f047&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
