---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WK2RB5XT%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFxL%2Fon6Si5Uguus8vCc6hzW4Z7ie%2FWC8pcbeUSvT%2BroAiBV6aVPnqi5Gpw7mlXfgH8UGLZs2mbOg5gtwo%2FgZKp6KCqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZ5yIhs2L1CPAC4xYKtwDT6cMKpS%2Fxh%2FuzAv0uQIShMsmLDH3kRBMPOoNdlVjHZtKzWEQIiAwQulL5YZmlK629EjkuWR3%2BBNePoLtU5ucjaWtew2BwKXMV31hTpafj4NjQ0RiBkWlXJPONCzV%2F5tOhDaX7b4NRYxfH6K2hnfqVW%2BdsuCzrabsCkaPtgh8mWkeYbu77dQh2RQIo21Yuk%2BVUR7MQG%2FH0aIyBgohf50lSM%2FK1FhUfmPLgwOJ0RdrGLvzdC25LWb8IGspX1l2KCTwtcM7Xrq36phtrUVS0WjkxzDxs8mo0FymAUU6g%2FbVanU2CXxVYiu4JVClzuxZ6iK4GCMsNUSg6tcCR5u3u%2FuO1xaZXvegMY39t2QEpcQr8k6IJfWuIEtH%2F1wLV2mpbwVD%2BxUdIsKA2021y%2BOWKD31I1sjyp21TmAzByik%2BhsUYxfG1IHr7O9pZzzfikY09f80IONP17sM9GhUhgWlHdMEGTdxrlC9EeT2VylgtFij%2Bg5rJmArJZoeAshpAENOOROSomiTWJTcEHsHTFlfCFaGfABv7nhTUm6QySPIvfsF7DOk%2BikJLApM2Cr%2FXHq1YJEiPmIxv8n4AaEGt9rCesAEk8PkSoyo%2BCM1GuhMnpowFYMswcHS%2BIms%2Ftypd8IwjYDYxgY6pgFncrQP0f7rOw6RKMMwuGFA61iX%2B%2F5YaB1bICJ7FmFOmjBP3HohP%2FOAL%2FJ9VB5Jlamqg%2FvjwJGD5F23DZV%2F5WtfIKeGTYGgfiYX52AMcnhRx5TyX%2BNI8cSf03TH9IS4foiT1bYFy9HeO0eBCKuKRKk9EHdFZ5EbLdGZm0Iy887Rewv9xHt8bgESxgm7OiA7OkejC3sUy%2FyF5JgKxpOqfg1RxtpHRnbk&X-Amz-Signature=7290f9f6d328f1e10fd04faeedd417da858ce5e0fabeea00f26b285e2e808e55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
