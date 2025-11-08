---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646O6XEJ5%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCIBzisrua47pFuS9kSuOU0byGeGD9GYmRd20MRMtJsISiAiBceDLbRaM7VU735lTxlcVIocJmsFmbcpjwCHnDV0IGAyqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3tUvf3QysK15CnDCKtwDIjKg6Wo%2FDs%2FYSmHb9zpx2tgF04wlM8EGJDiUEkL4xOVoSdw2khiMR%2B%2FA3%2BLlh19rutL9yYHMCs%2BQ2oQ%2Ffj4ur5DWk8dawgtmFH%2FKk8qdVQXuZQUKPxb%2FYCV446Ihq8WuvO8fq90D8VysY7LA%2Bwo03WWEVjJeEA6m3xXWLMNo00RgS8mTwByGnOENJ%2B3k9NpfuvQsj3Z0%2BeAYx1K6qS8%2Bo04PaFpW7gBX8sDRvmr5bV3XZl5whGBQdJF3Bew2SrMaPgJvmkHA2TwRRFsml2d3eA2fA2nMfC4MhRgUA9PYBMqlwNBDVcfTCEK5wlZd71tTCd7EZAW8kjD2SqD6j0vk89Zd3fb1NRa1FM6b9GloJJQSRspw5qJskx4%2BF%2Fu9R6wht8GRfMSaCAlDIJGCGVf9jAquFFXFvU0kgoEiXGzbEhmKxDTU8zEsq5fxpNBbQHHGYilRqI5BSZraOYwJ3Rd4uArPnY7OhMOD19cARW4zaNG5imU1XwFBiZQOVQm85u8U%2FRLYXyFPokTOrdwgFqoy7nFd0grz98zCu77TXxmYNk23vGl8SG6DM942m2qYN6gp1G4wQq6yU%2FEDuTFxiXlwGXTdKYMIeRFxdbSzfZAv3jFqDJK1QdBe%2BbG2VxswuP6%2ByAY6pgFHkwgQbxoN4NAK9fctXcqnrtuhBY0dpXfojtvD4vBITB3WgkIpp9M2EVaTZRQakqDqwUK1nAYPh8JQYDjYA3aPugg%2Bdwsj3PwoZUI0IvFES%2BX5daHLyEtKQGkivKHRzu25AfnwyRJIfdaYIBRS4O0zTbS5sbhaKNQUPv61O5vXbSymY1td5EuQN9CvSjjz55tBPzse%2Bv%2BeMnmN%2BjUQIU3a7iuOOnMm&X-Amz-Signature=fe59b3d3cf481cf8ef3da004fb8dbf9ba7cdeb4a5dc3fba44b9c90bb0b7839d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
