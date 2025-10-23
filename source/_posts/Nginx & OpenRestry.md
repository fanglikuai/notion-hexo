---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673NX6475%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T160059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFrOp1Mf78ctew1oIpSkWJwJiamwt8Hka%2B7X51QmY2kqAiAY9IucjdXF0EY5Ozpa%2BVxdBEXBCnxEZtch3QEn%2F3g9TCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMUp%2Fni9%2F%2B7UEq0DEKKtwDWWi0Y2KkxGbUHCvTN%2Bi4eUkfRnUWIrC%2F3OrXwVojlhcVQ7F5B0I7UKANQia3AVH6cYzkqIvoBFjmav7YK9AO9DYilbsvVqWoO41Qy5W0wKgWA9Tmaa3mM7RRZ%2BRXWg2%2B6x8mIH9a%2Fy%2BMe6lHBEv8FMgK4IL%2BZHBs2ecW5pC03p0%2F6OXQygEAFUoIcyLkQT7ySaVOLx%2Fd1JrBuM7qyCJ2lfDYgSZdXfCL%2FGFuqwH5X4nx6qZ64ANEk8V06eOb687m5bgvSoxi5vDKdQCemSzF7fszfqGkc%2BLTDDtVrnasK8QelFqoM%2BQp6jccItIc%2Beg9uPRxPsLF2tz8IWulVJCfovMj6Lar6TuYn6TaTFr%2F39M3zbci1XCKOUrqcBJZeMFoKZ4RU3%2F3pK2EV%2FIaiLAcCifdYkRtGFevmmyGdB%2Buhsz2zSJtDW46PJaeQSWy1zcyEBmhidc3bjFhB2vJhp8qk4T8mDqqHdfCpWac%2BurPPT5ea9EuPZxaHa1He5IDnMAl3s76ggYMqyPRb0M%2Ftrw6kOPY2p2orFtqmVdRjcDnGyVYfLhaxpKrrNsHNnTk8dbyELnYByLUCpQjpl2l6NquLN9eA9F2wjgEIcQjP8UZrqLYdSmkixDKHvUudfEw7pzpxwY6pgH2JJ6iWR6f0OkGqmGmhRElZkAkpRSgydge1G4DQNQk9Ph7DjjibKRIw4006rlGADbXoGsVRzZw69AaewkiGBy%2BEaDZScj6XdfMFFSMmwUdxn%2FlTxdTTO4nMFKB7qIU3oq%2F5VaEM8mQbSw2dluJ25ccUdAoS%2FNDMURkwvfAuB1caF0aNdH3hJGhfuwd9T6mM5DN4IpAGYcR98xO3S2uZ%2BotF6oYkdi0&X-Amz-Signature=965ae7dc8d396714f6ce1f2d857a818e1503078457615879972cfd31e6cd0bbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
