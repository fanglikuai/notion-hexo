---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFGPTL3Q%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDALyi0ZukRv3SRB%2FXyygHge9bzSHC9x25zGrMjKJJlGAiB4Rmk1aQk8yPPjNE3CVdCbh2oSv3QP4H2wxB5AgNL7iyqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZySmN5hkGqKUrobvKtwDKtbQ3OsGfDWMJjoMunRwqwNR8LGrcMnAnNroIpQQ%2FMjNV9bXqOk7uk9HA0KOw7B0BVy5MXp4KQq9U0V48UcclWRD%2Bh%2BvHxn3FQUrlFPCy377TNj2mLlgIpFq6EoPiHXPeADX5lt%2BvCpPF3AVzCsyi2xFCKZrrSQQS9lK9VPjYoGoDy2AEi66rN7prQASFhvlZja%2FrjvXIQ%2FrpogFMpBpHS6ChR2AtW1DTmtQWm5KBso6DrWTmQie2R7ncI2zZS%2Bfrv7yj8EmApcM2gE6JiiO0x3eyqTL9sAMctw950gGbS4JP%2FQtReKKnJuIcLGTs%2BJ9ZjESVp88AAu%2FJxKGzO3rmpte9wLhYAWw%2FPdQTIwzQQ6WjFDllOR3aakrr5jVTp0Qa%2F5eeYkIGFT%2B0ce2oLuNCssOW1sSTdBbIkfJCjivMADLd6Wh6YbdbArwHqp7RVAjK9N9gjYbqV1m5MzhhxuLs98aq9t%2BryLVFL4%2BTH3NiD%2FVMLjP6yuJObM3cxC3b5A1BzZqt7S09tiwU5I%2Bb7vsm5I477mBrZliuq6Cfebm9RHuwOzEDqMSv5czDbaqLlZaJwpC1waXalHZB%2FnQ6AYIaRskk3J63%2FYzgf050z3tgeXl5g59aRplepUGmkAw9dqtyAY6pgH9qWLrLffTj%2FsbfkuPNdU6dULq2Y0RaCN1qTLh6J26ktIVgtlSWM4wPp%2FEq4MMb3gYWhwPzPOtm5wYy%2BAIZfqFMPCu5oaqfMVTMQNTk2aSJnlj4eyST6g2dKgZaPfOd11iI6okuekejk5q95btKTpO1CSOOi%2B198jAmIqeuz5cLs4hMyykkvU%2BKFmcSD00K0J%2BZaHUlCQ%2Fiv0MeyediYWKEFuvQXJK&X-Amz-Signature=67b405a37253280ab451006ea82b96b8b1e66dbb8db0ef5ed3f554744ccd8482&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
