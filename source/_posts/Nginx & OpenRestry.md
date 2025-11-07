---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY6SXT33%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7yzze6knNx9BfcEwBXbPgq%2BhAbGkgj4knixG1%2BnxyHQIhAKQE5s5ccohyAap0CnlDe6Z3d3%2BtxBCmwYUQ15aZ%2B%2ByUKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww5J2eyFxDTU7vPOcq3APltRwR0C7%2BFeqEjjDol1u7T0jA2LSO%2FOqIhuKv5ubBhJ5nk6yNJt07Ajd84XjuS8rBqWeC9JroSU4%2FgBXBkKqXc665RFjGFBn9Uo%2Bf4ajNDcyvImUPKHfx0iBfe7OxHALp2%2FOKDN%2B3JUHd5eJA94O2LGmYHQfSH596474iZME1AW4ZT9O6Q1AKwtziVBZm2nA%2BK4GZjPl3dZl8SYS9rWyI3n7TAsSBzB86B3jAn0BYP9pg6kDI5f1iWBQ02XPnzkptgHx0BwfHRk2pXJm8hENBZ%2BdsEUNv6gsG89wRml1l8ssP2dojkK97%2FYxQDQ5%2B8qeroX%2FF%2BY9RoWJmrswPyMnU88nReSuPVqHsCjawa5vA8%2BYbVmrhL9B2zTCbgZbNC8jXztNmaDvrnLC6%2ByTydQ320JhU7abDBqzNQvRS7NVcHo%2F2WJZYrxYUFZcmy4U4EMYpiow7c6Yf3DJ9PEHz1EgRKrPXcPlicz8UEfunGc0aNy9nqUhpYPoqnDA0mKew0iGSQAwxdKSD%2FiluBEpU1tG6eXFqkzxWNcB%2FZ0nRk9LWnUmPQSmxeKvd2hVARZPz25OHJSXD%2FbrIe0zenUsMu7AwJksr8OOnIqvSGUmhwXdoh9KHcNh%2B4p38EcCZ7TC%2F67bIBjqkAVH77C%2FAP91cuEHZVyEVOGNRThMbVHm941jJy9VOXdBwW1c34YdtF%2B%2BrvqFPWAbBzqBYfWDh6EWr60RfhBAi9JRCDpp7U%2FQ1C%2BI0QENjtm2DIG%2FkAG0r5Tetbmi07TK1PM1WSP6UKirvMExG0hZAUxiM54sANZtgpyqim7GVS%2FCYcH1WA4OUVaC8vfKPpAooSsgQPhlOLgfI8wn4tUsdKLIdcuaG&X-Amz-Signature=751142daa87d0c823d4ed47ac4770032fc1d0b96dce9ef94f091dec2a0dd65c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
