---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RDC3MFB%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGUZyMGcDQ6D%2B1XyK%2FbIrbq6FYdtczM1%2BnP7TcF8B07gIgGKvPvcPV%2FUZYWxFlH%2FmNsbOddkJSUWDlfyNbA3Nbs90q%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDKn2pPwwCbhiCL9N4yrcA3v5liUcLNj%2FS2KZyd7Jy6ApCVXRYq5CZLMdsaZXBsivetp3Gm%2B5kfWj2fT57e80ewyuqfBFfCXr8jhE3dnQ4YE6PRU9ebKsPZQF0EzbINIy7q1PjsZvSKag%2FKp1GnDbbCMuG1cO8e9io8Lnf%2Fuf2cwF0oRFx82omo4COTUc2zMtJqsCGW7TkgupcO4MpkAe4jAeIXV91Qlcp5ZUKQGqDP%2FjrZpmUI1EozWi04G6S%2B5egCeD2r4GAwTNKn1RV1APAjLaewrc1unnUXcEK%2BGyMxptipJ4ORB2%2B8gQal1aLfs3n2Dk0nlWSJnKz%2B02b5gtV76vg3YVYmV3BepvxoiurGUR4ACd7r4MkI2joWY2%2FN5ez9jfSBVHTe7cAOvo6mjQ6i5YmY0FH32fQJt%2FRdGxfNtrIMCdwQm1mwaIprUaW02V8IviY0yKtNZTyhm20OciR011IoXLJkvuuqKSKSZloMeVqah9SMJYlNzzOIB6djq9T8zkSDxi2jTWFbt3qDdv5FhtwglaUBE2izn4oohz9L%2FmJFw1uUhys%2B7TNlUY64H4c%2FwdshaBp0b70bXfpDZPllF6uHWlW08KT4lFbMzjL2QUF8BbrLB2RO1X2X79QY2r0yVZcZn7xxCWhs0yMKbx%2FsYGOqUBUb6d3s3YQy9zcxdUFMeRGLmJtScUoqbXkpEInJYz9T6aMToEUDnU1iecPFgPvoTzCCULT87hRMDAOvCNE5xIWMAqayfRj4EjEjJoLl6FAxKSKdjhtPKwOYvoI0478zgGHKVnSaKP3AjByTS9BdyNPWKJXCRcY2E7OgrsumOvhd9WKNVa6YOaCO7xIyTEgaA7wqpx1OhPYed3T%2FHx4YCPGvyd%2FMC%2B&X-Amz-Signature=25a2388d8c2153c0f3f2fc422ffe4cc591fb0dfcf9fcdf51e95c0e18b6db4fd2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
