---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTDWRFU%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHArOKYJgB8noV1fLGKTHpETAL%2F20H2ojYN4EWHlEAvGAiArgnehEaq6osthLAVr%2F1CqNiTAch3Au0NqB0PHGoI0%2Fyr%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMaVTfrz%2BcuTeLUA9tKtwDhk00AmQJ2XE1TdFrebWpdbkEBZzarr0dBz7Ga3oWuqd8nVACGzkYhvkwIdJZamtP6rbWr6KnJKMgJa%2B4ccaqAwb%2BY%2FhXDE7OVyYLHCtE9S%2BgNJjohQjl3nH1Inp91zWT5vNzBUxMgMqzKDQr3gAcriuP5slEzJnyWXX5dp1fpldd9f2agb4CAnISWVE89PzupNYAPkNlufaEo7ggm0l71Zkk4M05ov%2FKlFZWbFjXcgvSZQ7vFkvRWhM1Lmm4BJuPFwGEuy0%2FMdzwyBs52cAtTRAWM9VrysLSg4iSlGBhvYQA8uaXr8fwmHBzGBuKhXiZRjHOFp9CGUfes9EKj9DJibC5HCNxp85sKUdKQhvYayXnxNrSI%2B4IUMY0sbQ5T2xkS2R11xnbYP2apJwYlvSc8zuLEizoMXIEjawg1vgrvLZFn5Jza6cxV8zHFaOV1JS2EHOWi1fY0GauMzShoIaRFrHdeXCWierX%2F5fsjdrVVjktheyi5UehVNbYuuQPMgN047TlV8%2BdEAVjZFiovu%2F52PTIb%2BO61nDqRySL%2BozxzId%2Bog6BMn%2B1TcmjFwnEbzKe9xtHpKH0Xpx%2FE48nDz7gsQ32NbTTGmvIZ%2BL1H6RCAgQuhYzhkVulZs0oGF4wnujRxgY6pgHJ0igsJ49t8Gk34WDCU82isTzOhcJ28s%2BWtjBJhn2UE%2FfEITiTwHwxSGonttFfL8zwXVe8Ukz6KP4nq6pqYN8ctQffguR0%2BV%2BU17EtCyS%2BBjggACY6eF05H0dMYUeoiiT5BGz1%2BQxnw70cORcthecDlQXYmyt%2BrZa7Nw%2FITCF0dkNojj2gF6ZpU1zPET4jpvtiOjWY9X4peh5DauWP5slkvGIVMZAk&X-Amz-Signature=fa2ff05f0253f0c10d091b8f1a5f188aca06de2d2f24029cab79c098e091f4e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
