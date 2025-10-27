---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YEFRI5X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGbmmsLvEzvsKsviuV12PYqTZ33CcOoAHoZn%2BYFyTFxfAiBmBoYFyEYDdEe%2BqVzDqtzbgoVP4WZRvVC13Xske3xBgyqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTryColROW7A9TPT9KtwD6VLRFD92nJa8H%2F8zkfL%2Fu6j3k03Ej4zLYCP1T9J4U5ia3ACryVOqeRHZms8p32Kc8otjWJQ63H4K6sItDX7PSXRx5DAUHXnhr9uE%2BUyq6tr7q3gmMGeJaXuMJxdgUZKrs7s7M%2FS2j6hSRx2hkliuUQWYAhK%2BriHjPsmBP64S72SBQdtU4iWugNFPW9q6XgCJ2Wt7MvXixJCJ6SVoqyWqqlzw%2FHKkwPDCxVIkmwY%2F7K6CxLwnDdHG3PQ5jcZzK61riBJ3JgpMu9ZvtCw145rIBE0GGYp1iS1X9QsJ%2Fu%2B0ZPbvkSRXgeetzjVwbcF6LPLB8%2FWJeyp%2ByhDjvqsh7N%2B9xu8ugf%2B3375W4vbJyEOOWemBQeuNajFVQvaawkY0UoY5z7LZq6FnIlHV4Kc%2BwbmXwD%2FFKbfNCDK8k2ush1Hy4zX9lb4yWN%2F%2BsCAgwgBLhxkEb%2BQKsMqPsGO0RB5%2F%2BUnGYdTyo6YIxwKp7HdrVUisD8SQTK2iVLB1qrr3Pb1msVeyOFP5OUNGOMxIqDBrrfINvI6lq4e7brdwuRyE2oB2T%2BzEJP85lCm4o4qrkSFOZ7YPU6mycRJtd9dHyg%2Bws7PjAvLomSMosxoDJmPmce7Zg7ZRSuxH0T4YyS3mr3Ywmvn6xwY6pgHocZPCEarwd3hVPHdLxoLk7ufRj7cVOAI2iqHNkW947rgfqqmI%2BxrlhlhmEdT1JfMe6ox8B2HJIQtnVk2OcgOKTTLKqzHhLFELhGHRp8jlUIug%2BgAe1k%2BtMVAcmDoKSUw5zgKy1KrNzLKioyH8Hc9MeZjGI4Pivgd8Hf77ZSPSc4HeN2FP5xKC7PjQ6Hp1P6sLDF6nl5vFdpceCpl9lXYXE7p5me1q&X-Amz-Signature=556bae415bcd21a912bf390f537cfe760e9de8e15cecd7eb81aee46def3db80a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
