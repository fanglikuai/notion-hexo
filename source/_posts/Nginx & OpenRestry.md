---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZWDUYYV%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDmsOu4ZireEJP%2FFAwvQmAWJhnTLYS3vVjWrGhhurb1kwIhAOme7wMopVOwJLQAk5r3moILh3km3cH1NeKOhp0XtmJZKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwdFuUq%2F15hYkGs3iAq3AOrQw19xB7E1p2qaW%2FIzC5sYE8HhjkHkn260BwPjY4ZzqbFLBJZZZ41AUsOX9Tny%2FPf3ZsE6iTjVgLPLAKimpauOXh9QoXkoJdwEuNQt0YJ32xCOGXmt7TCaxpF1fHvopcoMsNfVhX3wie9kKln2VkzT1IK%2BWK5FKjLSv5waSQhOJdmc%2BOZ7TO4IZiZNCU8%2FMGgT4JOYlafJFoHwTSCc6HS%2F5OnRO3psbhPfWMSblJPLK%2BiKBr0c%2FgmevANa10%2FdKSZS6KZ0u2%2BwawSMefZOO6A8z80%2BvrEYdkh42WSzlZz1QaAFMPr5QDu7EPODxJOeMQ%2FXBFhfxxju6L6F1GTd8usukp9JZujqSZ96VIry0kuoJ2JlfF1zM6t12LLOisltTItzOimE%2FYLFHXHzmPtZp8BmMZkg3OKvlgW7czMgC4b4P6jWHNQXDNex4koBaSy88n1mDXkNByu2EI8atztFl8Fg5NcTl916DiNIUlW19MIO%2BI4YZ7D0dpJW%2FzMJXyklbO%2B4a2K30p2azBB4Wit0nEBMsmKI937Cfikgeks9FpNxFGcFylTm5Czak%2FPgePkTCYDfQEs7Wazmr6HAugxNyagHHfWZXLLiyN%2BdqgDAQSTwNrKa5Txfn7jOG3y1zCs4t7GBjqkAaueWHz%2FpdLhIAObKdT7swPQcBZcmIgW44WzHy654%2BhEjxAy2t9gvUvB49vWPCu9lHMEiTzGytBy%2FWX3uwNtl4UOXtg%2F5LQe53i4sRn7gPpAnEw23Rde7r5gJ8dnXlnQV8GTon4WkZ%2ByMRvAJ7shik0qDS8ps7BUEG9w1dkdI1fzQPgLi1ZYNd1T%2BbbUzNz9lrm5Eogh1X7I8%2FJTnZtbHaStq8Vs&X-Amz-Signature=ccc53a74b769ba12c8866a74fdfa742b5bcfa9ccb3b90801e99efee05bfa16bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
