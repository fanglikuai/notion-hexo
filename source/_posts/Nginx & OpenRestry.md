---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ7OYEPR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAhNYZ9Q74PNOiIbYgXO5CJTzmNvr1LIfNCPThr1h3NJAiEAgX%2F7gNIQUKBFCYowq7fYZSHjbWXQ8qz8fHMrNNFHvekq%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDM369VTMy%2F6zA7AlICrcA4l8pChqZpJA5FaV2P9ecx4JbnWLyymaXLqe8LKWd4%2FJAXqhUdMGUPOvIijjRg%2FzCdlAkPWoz5cMpMi9KDsXnO7Ch6BGOE2Y1hXmIFaKUDrXNupueYC0wTdXnfdJWqx3ucPqIwUmZ228zHNqktPpaKK5fjxxzGnpormDaaS4UJGd%2Bllf6G0nR5xiv7ve6XA2s1qc%2B5qPZKVdduYqaE9vLOlPxymlq%2F5b%2FhuOtyupnb6CZAYkvUtU3oc0pvLSOACQ1R6MrEhvQaJa8mlzZdMBcyvYwrjyS1VsLuYCymJiky59sZiMErLkbtB%2BGLn6nkDzp9OoSuOfMqTjftYjOD9XjAjWT5zBZ0FM%2BGrMgbE6d719XDjKwzELH5xiGURqmYzl5%2BDcTWNLLD8vD%2FSSYkHhmi9nmyBxTn9zN%2BMBB6AZ3Thupb%2FU8IfwMz5RUjyjRSi6LC6hppT4vYhc%2B%2FXkP4Z%2FGJ2m8AvgGOpTmLeN8Q07CQ%2B68yoA8E7y8C8b6h%2FM1bqftj7C1Wla5%2BoNRidngFQ8yCDfCO%2F3BXaZqNlo6B5EJJggIHPCd48BEATds1HsGH4GC9%2FO4yDRlU7BwlwTC%2Bw4wjcAzXpUyLo5NCRd%2Fw1gSG5Hvz17EmPF%2BUiD%2BrbeMJu81cYGOqUBbNJ2ivZiP0605ZhM%2Byb%2FnIDYDmytH%2BNyEiVBt%2B8cW5gs6LXEog9PbiamvysGbMQFv4N%2FPplb0Ds2vTjQLacMTv%2Fs847o%2Fkr0xrZL7GrKUv2S4Ig7P3sVjHa0NIxESc86Vu7rNjSHVaLOFhA2GEMEC6DPkN2%2F19wBoMAXhD1hqZryGR2lxmRRLXyYKzXSyhbb70pfQKEvkO71HBL4%2BfQt0j%2BsYUO0&X-Amz-Signature=325228383c56dafcecf21034cb9ed108bf3b4cd6f5e94fd409a7c36300f434ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
