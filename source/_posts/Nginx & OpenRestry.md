---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FSQHBU2%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGJ1z9EWqxcGHLLGBqDhLUxLMXrMiwIpxqRfgLc%2BsvMxAiBjiVjQWzzRkMOS5C5ZzKqJOYtfzjDd8PWYs9qQxr1YQyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMNexi1HtTlhGuaPOFKtwDNJYwk8VrruTL4%2FJR9MFm%2FLBp4TrXhmJFQdo71TjyKw0P2AXx9Uvr%2BKkJo0JemqJtYV6QZHL4NzMMY9aP20vICThICCldvL3idPrINjrJsEQfw%2B1DhR7ZmruWyDjMD51ydIFXV%2Fzz9GPtgBsV6KYNcr%2BLWbRYUg3c4%2F4rGzLUUpZfGhgthfIqwiCU37%2F0yRBZS54ZKNEN9VLPpUrJX2oJpWyC1ouVU%2FeLgS70mH2uwO8A0dq1hVPQRK2bZYSb2E0Ab%2FqvK6P7CWMGS4QJgydtQUfV5Wr8Nxy9PN72WVphYppnjMnU0zS4Mh7n51vnmIJpJ5dqnqANZ2wZ49wTeSY9TMxWTMc%2F3yZ2QjqRLmaTnNVX1AqHnwF6YarMb3raaB4sk8z8TQqC3kNmFecxEIFOBUJkNEEb7fK4zGO6UrvlykFMP3LjUQpP8EcYqk82YdtGPNxwzdC0DWeUgBknODtNzj%2FKqt2di18AzVzs8ZQ71dHTg24%2Fy2b1b32TMhA3cPklq6mK5pRMsh6gsIvjEPwJ%2FkT7szFXMs8FsViSMwdjYUqh6TXnWEhGxPWmrFUr%2BvPhMNtqwiEoSbUWIqi4J%2F%2BP%2BTRkqrhwHAPurYeJK1ITXM1gWUQqpA4mDMNCQi0wjf7zxwY6pgGMIJ3N%2Bt1m1%2FpkUmXpx3zeivrAa3BYgZuLBv7TvHKpZuTJoDI0CXwhNu6lGb9IIro0tAimRtbAPTKm7DeppnJw17uXfFAM9jQDCRJGljNm6vx7PlHTZMh2GrYb2OK5YUx23jm2WZ6c0CvH%2BQShxMDchO%2Fg%2FYghoq96%2F%2B%2Fb3LZ%2F%2FWYDcE5SEr3XNkK81BQHARQR0ksDyHLwkzbEJDyTiS867tB2Bv1%2B&X-Amz-Signature=7e1394c1e89c8ea6662aff4492a54ae27c88de3fd944e10a4c324e22ca6ffad3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
