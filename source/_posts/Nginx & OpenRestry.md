---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMCMLX6C%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQDQS3anK24sX2RS%2FmDvfjYmxNkcupoI2tUozhR519iLAQIhAMHmE111hc62R1gxe8YsnxyGJMDFnRQd9Esd8hbjBsVqKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRE9R82%2BMHFvsJKaYq3ANt5D7VTloueVM%2FXA72aA0nnVjBCDtXBEvjrkjw881GKFAcYhiAeGlOZLD6Wboo3QNqOCqiIXgW3iFdAGB88Etc2a3MjWV%2FYheMhQMhGGVqA%2B7a1Va1sWNtqEDlSe8IHe7IRaqsnaWYt0ZZ%2FZWOXlPmvMimkdK7WdI2YV2LtZfgErHBCGdCkc8JwaIcdxcH2jkwXbR9%2BoP7WKgRUzEZM0Mpwu2lXu%2BIXGkdZ1RHy1A%2FsuSLyYEVMd73Tj67hOP%2BoTh%2BhmVGqr2jHbVjK%2BBae%2BWogcLUWF1JK8LJAKJVuYg4YuhOT53eGYUbwFFnfdsYmp%2B4p%2BLnGqR9knRFusggXEcnOzza0O3D7%2BMept9%2FEyWjg0XJJDGJWwewxg2%2BoEHiJuf5iWceLq%2BAYGAbW099D%2BLME9Ok2PqgpjBqAyHiq4%2BiTwpgJan1gPbJN5HrXvZtLm1mhMq3UWa3FUnNYlXEHpYWSY2iXDJvRRD8shI25Aulz%2FXa0zRa%2FNasAWp1gAaOuso18VHm3yAU8l52QHt0iGK6%2FT18latVpfrf%2FCsZUS%2FIVezLyIIG65mEDh0NH8Wm%2B3NqNQFkdbv7TZdq1nwNAuu0%2FwrUnSGscAeQVxLPpdC1kFiM%2Bon6DUH5MpUAoDCPsqHHBjqkAUWkl4J0KNc2BzXzSIyqDhgHdVOOKdvwNpGuWvz4f7xZaoYO0llkftwiS4kRSzMRZ3LngBNaRWm4knzi8BeOIgvNwOLNwU12R4ue5Wib7FMd%2FjPsz20SKQfJ32hae%2FIIGVZmMxOzrE0btYw%2B%2Frj4aJzB5GpKVmLZSo%2F4h24LXyGZzlgpzLqeoLQD%2FfHoc68x2tFXVHlx0M7fbcYWOKOhGc7Vo8Qi&X-Amz-Signature=8b0ef7f56c405a68a9b9fbf2ba67504062c19dfb71ab54c129605d49a2aa6000&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
