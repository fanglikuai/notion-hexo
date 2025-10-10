---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTR4YFJ2%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCpLyL1%2FV0iUD03ftgSvYCiweuLh0WUN1RiLvM4yH2pawIgHpNN4lWKXobcacB4zOWddtTTBZUceXu4%2FTDY0aaYxcMqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKbbJe8zykNIIDnboSrcA7p7cMEz3kbBAT1duCby3BIsWH3v%2BbMrsJjEwgLd%2BSJQuQ%2BksVQHFWT6zFVwUbSBIY9RbNqz7KratHpKL4%2BAGj5gEO%2B0Q%2FsXzuF4%2BA2mkBnD%2BQJsH47u3c88hcmWab7PXfDXzZUwPFyFPoglBHxG0CIUe5XqMzEI5tvtDGMk%2FD6d7o2y0Ao1atM3%2BotI4jY98JVfAuFeyP43s7ZmFi9KUT9%2FAXzVZ%2BNY1yXREj0k%2BC6VQvOvZKr4x5OdzYXPnJ%2FsDb%2BqYHO5jl3SD6P8NqBeV1luwXeIwZEf0bwt2e4y60dQ%2FI1qE0G20OZrcecx0GbsQS9s436tc36unifXCkZqGO7%2FxFlQkM8Wwi%2FehJvph7qFKMI8nwsMm%2F5u8otTTY7HxrmpvJCmKro2K1BUqmMIl%2B1DoOar3a61%2BtYWf9M1LWo5%2Fb%2BlYeAQIKhXpEHqVsfc14noU%2FZcYNbCx1pB3Tf7CuaaDBmnaEtdBNlJOw8J5iO6me7N301AwlTU2XxTHXS9zAukrhG0S93nvrv2peHJHO3O6D4sBdqMHTrLMQBzRrvfgFyXZ4%2FoObTA%2FGCFa1LSf9wa96RDB%2FRSvybTKhSAxzjMdCJ2KEs4aMIIhy7il193TGZ491Ca00MWe3JPMOSdpccGOqUBHNFyt1lg%2FP2gp5tBIVB688yQ9nAoxZEBeYO2BW7Abe2bK7tttARsmgfmKFbatlEXF3o4zupHUuaNPFlGM%2F%2BD0%2BO0aKFSC7XSBoitkMigIHSqsc6P3EDVJ%2BQc3can6OuOGUQbRHNyiVFRda7crjYZOMTnhKqOEdv7UR%2BUPwYV9IEnbYqjp3Vmx6x8Hj8gAWF%2Bmx2uQwQkFMGNbUa%2FWMAjSazD7ai4&X-Amz-Signature=231d3baf95174a1f359361f4bcabdba77c814d0ac7383fa0e52685ed0549a56c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
