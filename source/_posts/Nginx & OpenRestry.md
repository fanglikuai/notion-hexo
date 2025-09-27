---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SP256FNB%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIHSHD%2BLy5yHyJNMiXLiZk6rfdB2j%2FakJQtKa00qaFUzbAiEAwz5LJHRtcfou3sUJQOZPm2v26PN7Di%2BXPis4KzpaMFYqiAQIov%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNZZRZRYRIzhAbtUeyrcA5iTbrR9OvfAcsvjQDy9RvD%2FIIi7rIHS6xe3kc0VhIy4fA%2FG9AUyVKQ3q4TW%2BToqMWTdsfMhh72GWbgrfdmqTqwhBwjl%2B278iMGAZ4w6AbHvjiRo002T4S%2FIWV751EhnbJ9gGscGJ2lMdJYuFGU7agkLy5XgJTZ8trNNxKerLwy5YcAUAJ5%2BgZuBbjCi8ATmTfHN4DMsHDXKOAyMJk1VTvCJXwAyLEh5CC7TbQjUYufh8FDVwZ6RwSUcoVoMuw2J409X0Drm3r6fek5XTfZJ7wzNpQXDMGwyYaoUI8UDD%2FTQ98wwaUGG3gRqgJDNnUHqHRLpNGGSKihtHWb13FcCk1wQzcwCqzOJh3T84zLXEeFCL2hky6SBIv0gh%2BmLyhy9G31IMduk3H%2BPQ2obY%2BHk1p9C1MxG0rSVYl0q90OAM3zLBlp5kRKliwHTfT84d7x0LkxiJtOq5u%2FrII%2BcjDUz8rCGyHONgNEWV%2Fb19V%2BYT8wSZHi%2FGfMDqi%2FwY9HyJJHtx48K0g9Sfh3bTfqFtI6R2%2Bs0nDB5Wwhwy3d3tEIBl4Ry34Eub8KYJ1td3IdmS1X6s3D%2FYYxC6PfFUSNhVMUy3NmSgzaA0zFdtBVRkOymeJh3i1WtXb21MMAc2BiSMPTD3sYGOqUBiCiVVk2bXRYaqGzE%2F1Ya8a5dQyVC70XXVOK5ex33Y4W3v4IWLcrAwDljN9MQkZOTFWQEsinEuB%2FbrWryPLbuhOxZLsi4URWFVF2Ur%2BimAcifnIiVbZSqvLFwtJO4voUOXGCp3NsmyhLeZIQyuDArHmq2j95MMS081iM%2FOfk1yY8ygw3Eq59lwecuF6PcHf%2B2mV5tCp2pfwrNqheV3h4cQZWqoG%2FS&X-Amz-Signature=c86a70343f801e360bf617e8bd0ffa6eb573737e0efd68e9febbe70dc071c093&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
