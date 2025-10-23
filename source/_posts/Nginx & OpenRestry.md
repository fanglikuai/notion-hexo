---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LCDEETV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBv6w6n%2BR%2B8RImPW6K6KCtf9Mg%2BnzNOTt%2Fvw6r2gJe4%2FAiAPX32FADjYBx2Q76QGfpe43zlpvPqMAMC3gr%2FC3akqjyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIM7kHYbxCqpDxXThU2KtwDHv6ueFlc8m57AdAtA%2FNoWj%2FWOpy%2FRLPTkr%2BwIZEonqHXOhAlWr4Yq4%2FeN5LcKermvAWYhMibV4%2BtzY0nV5%2FlSuM5OyfUxmD29mIPO5p2Jri6wSRK9HyN7kpzyQZConkkk8BhqFL6DtLkFA3%2BRvOEBx%2Bng8yMhtDLBN%2F0VM2fiJJUiIKJ7ewwWtm4agWd81RDjjTCw3XyYNPN0G4q2xXzK%2B87KECd7dauX9O5Mf2FgPbk0eYGzXN61EmSZLmDCrZDHNzJrOH%2F0yqOblpYzGwRjEpzDkUlRXt3u5W2h6NW%2BUn00jjLmsxOXpKlC%2BR0hGzVlH7FswJGdlQePLM6MUtwtvMXh6pi0U%2FetIMVo0MhtrJaUb9SoEs2mUTir39a7VogcxTm%2FoigNmfoebH1dQ4ddCEzwY7QNoeOSjlz1v3TTmO0Gc7CTnCbCls87wQtVDmOhF6GqJ27ifj40w0JeevW0eUdGYGzIlhRWQcf%2Ff8k5HcZEjpCjd5TgewlJsBGUM6jGzIn9PZ8FZiwD51qtrGEx1Al5oedL%2BN5z6xiM4jbgD3Ap9VdiEwIlnGv9AJFUYHq8bbNrtKKrAoOFOagaUB69iaeuOltzgsYiSkltAtQlCXS%2Bj0w7hLCGn4A%2Bt0w4pzpxwY6pgHqT95uxPdlKvQV%2BX8xYea7g%2BLIkwViSTyyNeLuyrYx3LBbg9iut1VTGnM6p4HIsupuVq1pvGQmQUJoSbG9a%2F%2FVgi6IKrcaT2SsmT941BSt8FdqgZpS6K1F0w24qFhr7xJQ12yXm%2FV1MxafBZhARIunFdtBV5%2BOP036Sbts5Y5xmCrggBKFvHtLPPZscVcy4O98gZTJXYVjil%2B36iAveYZieNmha%2FFH&X-Amz-Signature=f8cb214e0a3aefa3d4df7ea81054e761039e610d36f3f28ebf3299ed8e6d3f39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
