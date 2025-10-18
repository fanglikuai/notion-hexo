---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WU5I6XDV%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJGMEQCIB82Hvsd0A0AdtiOoyWf%2BAXgeziz88GYXFZq5RHhocMwAiAJ0urcRrp7aF3pPQVwLZ6bDDz0ERPFDhATZWu5pCwQIiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FovMEpthTw%2FqMItcKtwDfIJ%2F9Zjk41%2B26UQ%2F%2BMIslwgQkYYlwCu2YjJnaaV1tSSw8%2BINl73R%2BYlJC6LXrdfLlAuhOe66B7GL92BGmqxfyB6BAClBKmAt7V9QuRnXzNzgdduPafcKZOJnDTKUMCfgXaDwGxzc1Yfef%2Bk4OmSe8Q5L7xN5uOjIc4OM%2BrB%2FYHyjQLXws0Ip%2Bte7Kd5LtUYp8b29gitj3inckCWsetJi%2BbiVHfFSfZ%2Fh5BB4NSpH6pnVOk4crBVSsnwJ23BJNWXIh5Zt0OIzyImX0u%2B5QIiJP2rNScgCNdGx%2BWKquii7NdsiHjDDEvUIxh%2FTPov2rgay%2BmeND8oNXCyu7nKtOLGMj1XRWvxcEhADEA1xvyP8tjilArSb0gmUOsX2cNHymlmxmUaCZgPo%2FGnIIa1ZK1gYrYzUNoMtPJgZqcMSfp7%2F4W%2FrQwR85Cuz0UvyBuuIdyoQs64ERphi9Rn%2ByYdOeI%2FXPLHXTKAan76KsZCNZWRK%2BFb1q%2F8B5VDqeWyWm8zovm%2FPSAh2t0Ahf5d7IoJuIaXq7H2y6Z450h0IN8DdhEi2E5A%2FJfcZuNespvCjSBhhXB1fvJugeSAaAW1mSThiJS5%2BAiLLaltmIW7ZGbf1sVUPaZPnAmkx6AoVurK%2Bs3Iw86PMxwY6pgEegSs88mSNil%2FFbtwC31sdU5ErXCFSxhMtvw5SKaJt15%2BNx0ujf9qIKwPUfAAblnperpqwMXkmfRiMiskCtTiovKE1F6kAftuhePiWFk%2FDvE44BO9UL7rHviEd9RwUQgajE2QDwp8jCTA0ne7KrC6hIxZQMfqmOWR6U3PTQSzQFvpuP5gEbH90zCHq5%2Fuaw8DUqzJUztSyzMsEg0mktDNDxUw8KrWC&X-Amz-Signature=456a53cf12da38b312c39feb15849c9abe57c770f66c94b4af998873cdc8bfc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
