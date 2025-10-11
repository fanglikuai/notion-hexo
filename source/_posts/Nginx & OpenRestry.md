---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJPRBO5K%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJGMEQCIEUhS3lwQrq6SMEYKMvyZa7PZFfZ%2F9mDSy%2FMz2qjoLUsAiANEzsnxicyxF4EivMnmPpcnGfsVVkDRL1hoAFCYE6lKiqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdjoYkdTjO4CAfa2EKtwDEpbP4bUYhaADqitFLfA4R%2BN7qVbahakaiTbG%2FnRld4IBKEWj6v5TRPeteN%2Fwm2icCevVZdsiR8XOaw%2FEaRxvuCUFX32Q1fyNvUQTbvxQgoq5KHie25wX4fcyAZdh3JSP%2BvxBW%2BGqJYJwSG70I9MDMc1ATfALw%2F%2Fd3w7Sl4FXS7fkZlKYVQVmwapGBKPM80kI1hyrI2rY68ohU24mYqqH7cjYGdRa2RXWNzj19otDcQuleXpGN0jck7FQMJLMtzQq84ZD%2FgekzUbN9LeoMhXQQ%2FkUojCzKR6smXisVMEHjmfYVrtIwCprlz5Fa9dSsfx2WbTzuf8TIXp6BzaAI7Fq5ht1RUSzY9i6W8OKDxi2A8gvdqk7G%2B1yPOEiRDAvh%2FFQS3mmUABE4BAoRiULdJI3JVWK0i%2Biz%2BNMWB9iF5f40GBQATIzyCkjRtFIglND35Z%2BRfu2xUNf397Fic2L9zy%2BMmGtHlX6IZ2NuqlAT1ivotnYPZq%2F8qSdh%2BuMl5DhEgZtzzpyYvRcNv7dQ3Ew8hzHwyepLc4Li3QDXahUEsn2QEQGpAZ1aS4OnA5mvPSXJPB%2Bi0U3PsrU9NTeFbsr42%2B4hhXAcz7RXLZwihzkOSyII9tVeqzUgZBGG6oBge8w%2FMCmxwY6pgEynj4Z4gCAPqKZrr86dE9m3eL3eRCaeLAe0c9bQIN5TBona5XIxyxH7E5Bl6wXZq2I2%2FQWzS4InCtj%2BHnvPXd8Z%2FQeUHy4EUcGIWJWD3OYA5Caqs6mcp54%2FYbVCTe53WMETPXJW8Qtmm4014gX7JTUWC9vwN5sDD39QpQ54%2Btfn2qNmbcCj3iTE5qdtSgyUdTgsnnPPODRZrCyKs%2FmPzXmPNqyDiTL&X-Amz-Signature=536373a5de881128113efe99d9dd944841474e05b36eef4873c9a7a8420b7f1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
