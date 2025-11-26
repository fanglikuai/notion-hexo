---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663DDSRSW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T060054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICb9OQZllM3vj5dLXQt5IH5vXumy2q5FdN6Pkc8N130YAiEAizGyrMPXgMJ7qGWalBfqAueq%2B31XPew0K7HKYJbhwHoq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDM5aPMjexthEs0FXVSrcAzpFZEsgpy%2BXfOk8lxv42TVCWL%2B%2FpIdFo7OBw8I7QrYBersU%2FYYSnx83AiBXMXidYVOHAUbXadW9GXtxJFmRMcmHn07c09YcgV80M3lb2cl83tYZwjfSLwT5NvXjcatfe2OdckcjvyGtFAPSOJoE1nANP1FL6AjauSJr5NIKhESY1LRHRyUEFF1vG67N9BpaYdA%2FZYsV5hGvIbLWlwBVmi8w0cNyp6gWevwX%2Ba8NrOp%2B%2BcbYhyMrt4ML1G8XGbBpqOMOI92O8h1HmY1Qk%2Fre5OttbGPXoLdoBGFO60QaDQIYBWlWP2uzS7hmjQArL%2Fafq7c%2BkfSDMsb97AIGqJgAS4YS3mJh2XpcMlC7vAivw25vUCCNTOoSerLKzBptaFYf6Rq2PKGRF0dUsJHu0jbLt8Aq0xYGcix8TnkQv2ZhmYpACPWOkWu9dMy82Py6jVThpBFXzH7XzJts581COiUzQohyRXDia5eKg0ZiVg1rPSvjmPa98hifFLoUPVOjGdFYaMrPcy4aMLLFOs7dB5NA0yJ07ef8TVdBpV6iuqz7qxlGQHl%2F3JFYwdwsNZ7zfJg3dLmt%2BokV%2BAX7WQ09uNiesscLEQbim0kpqTr4CZvDeIuFCcadVYL9iG0B%2F5HoMLCWmskGOqUBPu2MfhPVfPgR9QHA2SqWxdbO5nR2zonwXTEgHVb1MnnW9WQ%2B9Q4EHyCCtTpUQk1peKAyvfCVVImWJ6FsnlXhnCDI4v2nrohjHmIQo8Gtzmz51ANsCf%2FGNjjOViz4tjRcZ2rEM16L2ldjIQz%2BWUcWpACsQOuB9B3Nunkg%2Fqw2e7Z4uNubcbExYL391lYUoxPaswhc3qcUVxbmGrlbRmSpg1MEeqsQ&X-Amz-Signature=f3a4efdd64c92fa685e3e4f74d58f1f3e74401c456f78c71fd7eb2b96a714ac4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
