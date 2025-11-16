---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNLQNTN5%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeOGFKs%2BGYEbv8zif0T6%2FbxLyYaVoMhcsrvBM9h23m9AiBAgFIYq3ZYGz3c0ekWdH5054A7g4GDrbgkOeSb9wal3CqIBAif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjd040Y9kDEQHHQYKtwDXl3OJq2VRovpEgfmC21G9XsAGEeqGxsVSruqATLMXqNtzimB28%2Bp7xiCBqqPxqBooFSPxERQEOGs40a7tJeZb1oIIr5Ub8Oj1T7LFlytURD0BmsAlUMMZhIMVofMWXOD2jMUPKtatOOo%2BDe9kCNhchs%2FzF7RCdgaSDw8i5zxILidCzQIu8e6CEagC5vCgUBExR6DqekdWzNSaFvySrlw4%2FAV2b0KHeugWH8ykigBVmOaO2vOnMqAWRL6ksOh6FIDzv%2BGX1XnjC8X4XwyaZTkd5FJrjSyYx5vBhJUstQXRIQsh%2BVDr8SVs9vzWA1ubX%2BU94%2BFm9IT3eyGMjyKVpPBdZ8GcrR4bY1YW9lR9la9u7%2FylEZkifJ5lzhmkfNNtrnedE%2FtX6hyNnSuHyK9QROLlV3uCPyOmKLT2g81FLL4lT3f4FdewvkJQoxTt1ROW917e70zXDYcyPp9i86PhJkZQV9Ll4%2BvtN9ksA1NysyOe0u3VA%2BPtTlweWccqKhOGoJqbyoX87XvlASBxmM%2FVMR3mIasvOBrzy9%2F8LR5nJxkPbKQ7Ogv4yGDwrdStjpps2o9HQt2RFAEFSu3K%2Fzq9x%2FG8bX%2BETWovLhy5OChqBGHJP1uXO0ao7kNvCZnHTQw0ZHpyAY6pgF4NYbzgYqZ%2FISo9I3kRiX8%2FSJzypn%2F19jxwyYCPGcIs8yQUeaFgnryuwFSqqelh0n%2BmeuVNCFznbH%2BwKvvtmrKVXhq%2BHxseXP4pRJT3YgF0gOvUxlCc2%2Fp54o7Y98a4JGE4SAt%2B38ebmbK0g3YzaZq8W2kioSX7CfyqKiVxc4vLgFVRpHZQbwKQTe%2BNsAg%2FCi1EXKYZBedsOxB4L4W1ZKHb8OY1yI3&X-Amz-Signature=68598de1919da4bf39b00cd9553ee53c1ad8abed1affc7ffc760f655f8d7f6c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
