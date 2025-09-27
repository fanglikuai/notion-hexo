---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHZLLJQF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJHMEUCIQCQukkTyBoUYDCgFASMWelq186Bz1%2B5xYV7z8PsCXDD%2FwIgU2DWkKMHdsVSU1GVv1PxrD4BLaNRTrcatIGvgid1w0oqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLAIrdt50eXDBPcW%2BCrcA%2B5AYb79hPfIan3vyDfND9wB%2BOmb3ZLp4uzIgGc2YpL3XyPlINJS614C62CsCf%2FQBuArJgbOQHUfg%2FlHfimDGPHjUOdL0WXdTFnqwBXUGLYqoTmi2IYU9zwpUAh6%2Faqa75b61ZOU2sYtrYCPS2XVb6KDL8rNH2bjtOVSRanZj9eGJk2eQsUssXvG7ef2OU5Cr6EmDDztqtOklPwn9wbNzXb%2Fym%2FQidI5OwaVn7O9SFIt3yW%2BpCrRNKup8VgvYjcoiRqd4j3esXo03a%2FJDRsqK8L3IgQ32JmLpEM8%2F6Sb2T6PTzZ8gquMzo71dwhSipbzkmltAEgso1pqZ4H4s%2BVP3Nk9QYPCZAgwdde5m3i3GMwk38bj3rWAvT7cF2Vcac1q9CbzrTJPytT11FY%2FCNy4HrfJ5EE9zXO4bg7vvjOAsnTRT3D3UMCAtDNmlsY42NcjZ67TkIYxWttXc9i18l1GcnSt4KSi7jrUgCPupztX4i%2BAxRtbbLaLc5FPUmIa2gesn35UCz8ptNO3ylXE6g0HTegGTcGuA6oc4NfSFT7Qs1Xfrq1FItSXsnt4lphzVzdnyw2Bsb8qrUr1upC%2FLPAlWlxYHMmjwODsH1RO%2ByU9RLJJ1V%2FpxM%2FYsRoWcce3MPyp4cYGOqUByaUnIOXK6xLfWJq4qNrjlmxF92KER395RHzuWw%2Fw4ucqdZSKsE0Y2nWX%2Fi7y5aj7t2PQOQYkzu9t%2BFFFfkq4yoFk1zkXrtwcguTd44xX7mjD%2Baq%2BkVbhpTsJdkqlRh6K%2BoHc%2FYKhY2Qorwnd7pA6cZQwFMCUdxHmoUiXOOi9BzFpuvNLxynun1ezCALgwLFwa6DoBuAepnPaF8WVsNc4LcqQ2JI7&X-Amz-Signature=9ef9f2aa63b208af8e3765643a0c7a81dadf7c0261a9b80c8a5622528e3f3152&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
