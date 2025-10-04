---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZOXS36%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T130118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDIYiOi8gA4oKBcc2y%2FjLvtw%2F0TifxUjiTLUfQP05k1yAiAJKiBy5jFm2WAhrJKzSo0e2S2z3xafrf8vA4A8ZZ8hISr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMfb6p%2FpDNCYwHiyWrKtwDTpJ4PwDK21NNbtcuFL6I9iCdKoakLpgQpbTFs5icrSWtNTaV0EvfYdRtWeDefVsbFcbMle4awynPGc4xhRZ2b0THYe4uIyn%2BguHhYI%2FdmpU6TFPkB2xQN0f%2BGMhNxEd%2FVbCZ5xqNBJgBorgNS6SFmJNzAoJyj2AlXefqd7TGj5r5%2BR78qrEemhPPaq02Dc%2BwhTowRK6JRqnrwrGal%2BsjwFnRs4wf7n65DMHb9mTNWVhJw7KtT3xZsfYoZd1TeyPvsineBXcSlasvdCswOvEfVItJpZXfXmm%2Bz6ovKW9%2BMluh8R%2FCozD5dnx8V7jwd9Lr61Fl%2B16OVpij%2BVVzrr1gItM2G500FYQHTHtEl6WBPYmqzu7EYWKYGk1xA4J0jLaWcrtZGm3EVKHuWNJRZDD5YbbuKpnP3P4ln5SpFcIFZsRINc2esx4zXbFbG1cZTuF%2FdMQ1BH3xRnEF0n2l%2FZ6EqUfFri5VA5Lxgl%2Fqp%2FVOrRoDVhVeG1t2xRjechQIazB5fSgzHfcKFdgkaKmBnRxBA3EkuYHw8KyJU73yfosvU4wd%2BxFJF%2F%2F2vGTWr2T%2Fyq0gYIpDtHC8hknWVhAXZKk4iuysdc%2BLW2ymuYHXjvNV7kMQxqatMUKMmzzR%2BJwww%2BCDxwY6pgEfzW4bbgS55QlSXgpPZqKGCqhpAKnSujSfUqhxhG0wrhtFwAsaFq5EDT9V94s1JKvSxBlJ7CFIocW12lhG0qeSmq84NqPy%2Bh4zfBAI375B2KrqYS68ZceJ%2FXe%2Bi1HyJSRYis6tIjrVY6fJpbIbLBi%2FrhzEXgselPxdOIHTT0OWBLs7ZRWjEsXhI7VO6leqqCsj92vI91phpyVXi7O7R2QRQYisJasU&X-Amz-Signature=9fae80328c7a3cc5158164ceadcb370dc427f27eab08c6efecc7549e47cfb545&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
