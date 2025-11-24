---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q62CNWDT%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHHH7yxhjL9kOZOS%2BOjzDUN2KSOSjggzUEaCi6OC%2BuKdAiAOH68d1l7UwBjB1Kdd2wxvN4zS8rCg4M%2BBuvSVYMGODyr%2FAwhXEAAaDDYzNzQyMzE4MzgwNSIMg0CWq%2FVC%2BzHQ8vrIKtwDKrK%2Fe7eaPxQQWYVslNebmK3klJkAyASv%2BPDCNxuDRcfQGcbW4jYzL%2BxiD%2FnkK0Abouu%2FHfkDhV%2FJV%2Ffc7d%2FfBqrKhcctSa5AOgABVx1qnLh3hyuRAa1Ls63FczGpNYNMZMQuhzJc6tP%2BjOrzvny3usZSPtFU8OUR%2BbJTnz%2BqUO7gq0B72%2B5gTxmdIpWMHl%2B9bbNZLBBXm2Hhg6UstJNhPN7mz0vZc7xR8MABvmNLt7ArmZFH5nXndwEetzNjDInYGncZpK4X1Fw%2F%2BQ4g5WJk93KFjJBW0bYJEa4%2BLEMks0JFBnkfJwdnlqg27i%2Fps0uoSqb%2Bhll6AQ0v667VBIPqCOSRuaCxS%2FM0%2B81kdk2q937q8Mva7APomq1AwgCNi7S6%2B%2F%2FoqUTl9H6EHAS8xNLmFrDJ6%2B13OQukNSLQKzOYTtOGOM7YQJvsQGoZGnkyuWf9kRsjUaKdELlrbaq67BQnl5HdeMNkU0p5UIcGO7mfkwrPmjkVcU8eO%2B5q2dtgy%2FqITiDZQ0nK0hm6J1cZvhXc31kjjQf7Q7duajZYWBAy%2FrH0rCEIZga3nhTS419n6QNJsyL%2FJviUmYEBV0a356K4zZnpbmKhYvIw%2BAsM9lUg%2F%2BAhyjeQCV%2BpTEOXUrIwvr6RyQY6pgHI0S%2Bt%2BTNYwrLO2BakGF2pFbLnfLvi91pZUQWeNUqYAmF1O3Ts%2BbWlQGwdF%2BJi9JEhD97KLHz6haMQBihA4lDSzL%2FVF70AtsmEMsChs9PwSeeRrWEXLddQ763rmUDkCxMBNfFRIktcm9q0An2XfrQJ4FXhFO40hJ2CftZ55%2FGdjZmGkmcVJSD4J%2B6P4qnlu7RatzPzQA9jguyflGYYePVYHQKD81sm&X-Amz-Signature=b4fdf3392f6c3f52b039d0797815d63ab9a7b3a0bf8d8232f24e45b1f0fbc075&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
