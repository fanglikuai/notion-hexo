---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVUUNRA3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDNAZxABxkW2snBI8y6IglOLNqHxq6jWweU7JWGsxCnCwIhAOpKwOivFEIfT48j7KlZtRZeYuVYn00oHeb4ECeetevnKogECOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0UvF3UdZVJ2nXq2Iq3AN8kOSWzCb0hc7wFonIQ6PTiecOcwjDs2kiRXJhi7dw8AREWAKspTTtkoiY0aVwzpC1cH3bPBh0GX98A49LAsBY%2BI0oHPthz5s2nJYZXtdy1yzZ8PVY4A0uTbMV87kXMNWo%2B%2F87hdaizAK9Ug9d5Q9XzA%2B4kAiBv5SXo7nL%2BU8FmKS9rqZCiKH6G1wRF8ukW8cfxiXO1Wo1CO%2FglZ2WJFQ8UKiLLxKgCZQbRE94D%2Bg6hc1MgLONybeZnouXSvwydjdVQTDsTLHbQnkmhUtvb%2FOs0RkYkpGK0XXlXwAzWfOKnlbIChUVI1FjkcpZR%2BVm2QUCHBSQkP9AvmDM20Ost7OhAfPQEqG1EfUbKBLcp0K8sIHkfwpnPoQlGN77XT%2FWYkQzNTgu%2BssP2ocaraeXFhzqyZMI0UFvFDyQBCK7gBEZ76Ps4GHKIMo7YcgFbOC2jwG9GDSecLjKy%2Fwyz23kP0BEKOSNkmzFZMQ%2BdQm4ArqdDC0YInWyde1ZGObshgWJhqq8dN2ddOqyu1dpWOZ%2B4ClPTYi5jFeIF30BxJxf0WFQSl3%2FPToO9sfQmsbvNZwmWq3DlXcF8YJWO7PssoBwv4TkFlmV5W8D3B%2BZCYeamVacwqW%2BP6CEbCjfvSVxSjDy1YzIBjqkAZ9DdZuP2dkDHssuKzIs7G8TIpQ4AQ2cXstDln18OhCNcI3EE6r1FcTjXi3nmfnIwFfLIQIvFm6V7bpRYy1BswrbUB23l2YIrMvp8w21w%2BZ4k%2BnrBwrxVxOXcmLS7lsueHxX4h9GIqQETncsipCovIEmbEUSws6qYSCYSToKy78wuNIQtJ8mGvvuYC41UWUnb0Q9Y7LcA%2Fdcs109UKLQYN6zW1On&X-Amz-Signature=52ada072aff7f44da47a852f415995e6dbb5de96da8a3703e9f06cbd209a2372&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
