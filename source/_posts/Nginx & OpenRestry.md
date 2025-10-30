---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN5L2GAT%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIQDwob59HAtk7OE3ZifVLoRZfmhPaitXAjHSuTExPo%2BTewIgDWhuzlUsJiG8IVHOFkuBu4eHcqZ21MzcT62XCij5Q5YqiAQI9v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCGwqWxJj8NpTLMKLircAwxHLY7M3%2B5wisboI%2BvJ7mivXMO8KDmnRQhMMugIZkaXDhf6lMiIky4tA0E32lMKQejndEWqCnkaa4NiAJ2danBtaZA%2Fxtwe2alqlrj6TYxZHTDxsMwX3cwTh9X4%2FMOoB1CMkDbRKDlIhJseCOixOwX%2BI80ND1LoH1P%2FSdkm9azYjnTCZIv9sqJwF1iPB7V%2BDs%2BFdYvu4IdtrCzTniNOGsyCw4d3SKHLw%2BWBIbAZ9DmV1YMLq9hdhy8J%2F0tdE5d%2F39c20ce1U7wdltxj%2F5NR7sLAaJleDkYAzfjtkL5TO7dfvoDePb4ybEWZEtctOKmPramc0UNXttgaIRjSRIg8L5ebrl0KwqdqDJ8oySOsjCYOyjDYqYmBky%2BuMG46t1cWSy8Jc5gl5B2N99hFQwbrDDR1eLpjCWTrDHjBSGJMpyzCZZEzrijmq4PwPEPHbdnL4baLi7R7jxTwvs49clXnRmsutVGCpOXhuld6S%2FPvHBrZiokUDJL8i%2BvXrY8UJ7nnIg5C8otpqXXzo53%2Br%2BqE9g6EmwWd8iQ%2BBI0ErG2MiECjwHMn6OlgfWXUBd7VWDlfFARWs7EEB2gE4hpmtCHohiQfw7lSK2QLtoopveu8xXOY7gO1RJengrS5yWByMJaqj8gGOqUBJ5n6t%2BXIEXwZhwHCSEoxWgxLNYHS5N%2Bak6t0wCrZQOvYWAy64Qz8c3S%2BV3BnT%2BoBRQVTNNamzCMLnYQwz4RMQ8nDHaUQdHWM7YAvpZEWVit7hLT%2FceheObCrkHvtMqMRf1moXp%2BuzNw0e5hyviztF9ZrxxL8haukXc0cQbPnsiyZ%2BAubZtKC1zfvSz9VP1iRjJPQrILqd9zkm2wu0TB4Cl0IWyBA&X-Amz-Signature=e833201bdea446bed1e8aef7daf21cbe708926834bbbc938b7a66243735c0916&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
