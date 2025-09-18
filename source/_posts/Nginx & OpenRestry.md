---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WYFWDE7%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDYkXq8zpBvPCnXoHImTc%2FbPcZWLg3qdPW8BMyH%2BwQXMgIhAKjZd5fZXb1ohOHIUTBAjNsTPK2RBn2mXdFETyWsE2M9KogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyQgiwQt4xE4ZG%2B1jIq3AOJ6FtyuKzEUqaPyHMLNDofGdERUiqKrshfTWrIOLG%2FD6b1H48uwRMu2WKL1zkOY3NyMto7zTnPCIDODb65ZEbcexILioqc%2Bt3Xke35WCazNmF7W%2FiBV2%2FHCDdd%2Fx9KPcRJ6yMUr%2FN2I5tU6au%2FcERN%2FDEJvuKmXN1UYa7t4zUxjQBP60XjndRxsfztqHA5O4TwDB9uMg9RqlJl5Vu2GKoxhwSMAoHwmf3AQWwTYKKnQThJ8RZ4KQd20XGpu08OtY%2FP3l5hOkCdhMOkoIfi82Jc6%2FIdfkg94JAeZsxfUoJukYooF8XYZlKelPWvEupBGHawbeBFEfL%2B%2BWn4ORFyRpQN9i0G3seR8tGlXJ6U2C5nrtHnZOxf08oDxcCBOiAnyT78%2FpvWorcJvhV%2BaBpPm8QUa24Nyyq9ghXHNMUn906iVJgKOQjWjkfBQnPgCVyp6980FPHt5IDwkqRHp04m8rxhRUCZb2CxppZ%2BTjzjvLh5Oozrzb9o2J0EqqY%2FL8CTQN0g266v35lkcsYom7aqfQMLyPKC5G5q2tWNANDiqvL7XA89nHFYAjnZv4pMg%2B3VhwUtHNC5%2BNTkbhW88saVeVqnF1AmzZ2SNCV9z7Vus9marU%2B0HNOqFtAzMMfVnDD4ma3GBjqkATx1duuoGEkjSWu%2F9AAghAtTUxF79xqgR7OlXLtx%2F4Z8Y2iTeYqQch0TIQaQakgYNXm6%2F86rFTOWrjpChOvnHGz49ZeynruI5nATeOsdRm5GY2xZId6S0p0yLoszs%2BUDWe5fMefZ6AruuiYGML0u3SQVdCMi9sOKsXx%2F1xBee3ts%2FhhqHLR0JF9ApwZ%2FC%2FZR316Asf%2FATVg5e%2F6mgPVXJqffTSFu&X-Amz-Signature=076e7a81c217d8a247ad552307aba48826c650e027099d063166d271fa6ae1d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
