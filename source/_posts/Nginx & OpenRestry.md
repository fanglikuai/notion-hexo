---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TEP5YOQ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDst0M%2FwsLZxb6nEr5UEHzJYL4hvdQ1d%2BJQzUHtyLbffAIhAIehHpFPM5mHUiwsvhl7qCLbSOnru4L2LJUUoTHbwk4rKv8DCD4QABoMNjM3NDIzMTgzODA1IgyTpG%2BeYXrXryms7qIq3ANJbNg2IyhcNXu6rFkDsfPaadJ1%2FY6ZpcMaV3kwNbgPARtWOiNDjEKqBUxdpTQciCWbdL%2BTxlsILafnYaCBdEIjSumQ%2BuCOQVRW1HVNqiA7%2BF8u3gl64EW9ukbm8oI8o5Vt3JlYt%2Bg%2F7Xb7q%2BMdpvzeLmF71ritO%2FmWAl3BXM0mMNFi9QFzMRtZsyBWZeMZrc2lVzj9LaAfEOHlsvFT5yj4Zl9N6h3ORavcIy%2FDnYfXWlkPGTvhC9OJIz0zlL1h%2BNiJXdJt%2BNMcIzMIo0rPS9WC6dhAOzFpCGtJld9Qqprvv3tlXUGOAVt32txguEkuYnxmlblwgyE85HQsCxtyv2Y5ThlXXvSmVyXgFwjjmFayCnGHM68rvOQ6M7EaNTUziGd9Z942A4zFCQ%2BxYx9pmRV3JdG38GLqBnznm88lRiXHVj8ueIGA0Hr%2BVTq%2FJIlHabw99gbuR88tbTuvVOIhpkCOe1u5Or5dS3byyKhPA6SBomW4TKIyXaAp9%2BwvSJVK4pu%2B1AAv3%2BleCHa8k%2Bzu4jmBmuPGhP358SaERsOM%2FDJrvBOTxHkFuFQXZsmNKPnZNctu98SICnYlu%2FcvTG%2BfFa0%2FSWcSvE%2FcxpZLCIcWvd6UES9I1%2BjZuRktzzvyMjCXrP3GBjqkAX31CzigmhpWZC7xiJdTviyZwT3qlWz27FFCT9ROA0pvI7VQYJ%2Bip38DcSE3VVtcNTKO%2BXqlkTj15VockyOJrqibKDYa%2FVgHqeReJHvqvXjLyG4p59pqpigbhVWGkC496SmdWlJ%2BBYj4W9DiIqj%2FqHQsUYE1vOuo4YbSH1SS4EmPNlYXppTz3eka9zetmVuWlICs3KpIVPatWbPAEkEyvRQOq75m&X-Amz-Signature=ee24f08cbc94b42e03832e3356d9f71c769cb24bf908b50f86dde867a194669f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
