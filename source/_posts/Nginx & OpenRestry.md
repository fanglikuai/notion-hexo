---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KPL6U2T%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T160047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCr%2B1tqi62UTkF45fQj4cKmADta%2B5eUNxmoOzBjIVjr6gIhAId8r3dKNao5feqD24eN9JMBcW9LCNlLx7mKCLZurOTVKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9R4hi%2FVy1ykqHVE4q3AM2JPHOwYfRpgXhvEx%2Fgj2JDMVe7HLApTn4fLgn3%2F2xdqML1kYPYK6qfZ7ONxjCd%2Bv%2F2f%2FPWeM35D4uY8yqhghyCrT41INzyNZNjumzG9la8kah%2BTm7abruIDvdjOxmwt%2BRuWvoEtC7C00lqfYb9FpEj7%2BzKA4YeoLMdt%2BlBPeZ4%2BXCcXi4Y4ysH7ZrtCPus%2BYYg1TTejtFSwn6ywP9pr7hbfxrI741y7Avy%2BKkOyBPEjQylY%2BkI%2FzW6%2FUoeW3yYs%2Fu7dUEcmqk429HZkE4mWDHWo2t02nIKyMi%2F7C5XJTfPmgzlHzHgOsFDvUuUI6iwLleoUhhZdQvXaVHR6t%2B45Evk3CN%2B1onBwGc1BaZgU7gdy561UaMHMKvJm4JzgYZOewHGmw8FAMJxm1wDraG02eWn%2FmJzq2NQhA3iF60gmQhyB9NcInhTkjX56vVSaXfgL7iLInG31lvzIE%2FX2QB4L6YRTyAsSlvJnZvU6eSqUIdRBeJR7k4Rs60ukXndty3jjHAB18eaVUUaLt8NQhqjV8SwTspNtiCue22KikoRhMtoY6Pm5Y1Hu%2FLZJ5JRUZJzXzCQHnZHeM0DLJbtdXoxz%2Bnuq1WYDl4zncv2Yv%2FkFCAyoiw6BKUCpqQr%2B%2FFwjDUssLIBjqkAZplMxEZ14RV%2FsulJTL8jf%2BCTAzg0mowy99jkpiyjNNesX4byEZJ2LTIzYiH4c1y9EO1xGz2NixrG9BGi4wsxX09PWAKnFWqxjdB080v5DZmT3g5r89Jx%2F2JtBlK0ZzGwrjm%2B%2BJdKgJYdMd5Ob3MIx6WOGVr5Zen1n5OzN2r7yn%2Ft6Amwu8pH8bouwPUGaqvlIPSCMf8HzQYFdAK0cY0Yl5AO27s&X-Amz-Signature=9a87294905408c90d1acbc87c3bd046c725f8bece470dc336f8f4b2584001ac6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
