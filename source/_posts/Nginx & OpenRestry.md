---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VSIV3DU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJIMEYCIQDOS%2Fla9JsW%2BWxoArncpraXXQvmD0tFfq%2BcOdGzRC2XZAIhAOEHEdFfVq%2BKOdz%2BDW2FQDhz3ZdYaltBa01Ckq4ptdqCKv8DCBsQABoMNjM3NDIzMTgzODA1IgzQ8je%2FvC3aFuIyLIQq3AMoUlKQDuXIoDnbeMFd49jG6gUahMhmM6%2BVW5T0Szh0h6%2FCAoJMMHPs2EZ%2BCNdtscA%2F3WKQRT9sJwaNUyWWLsypVRZCvJO8bMNjJ1Ct6PQEby3NOh3sLdWsuWfr4DXjXPNKzhXXzzPHrPrendAhnt8cdp7DALR%2FngCkTzQzay%2FUbk%2Fphfe8ensdHCMjr5Cwzxtmszk4nzBAjxwBuKCoY0Eo3L%2F5sYb8afuahFvY9akR4AbewfbQbX6LSpqwW2pcfcZ5vgV1CJP6q03jQeBWx%2Bhw63Nd5PTMIYsF3b6q5TvjxbVSlqERQZ6Qma4rw30FjGpxcnZoG%2B9IAqplMAr5JTKm5%2Faoqm62Sc4ISvGD9Zu7C09%2F9la2eGqshXH3Gh5nAVstnGGu4TGQE2bqJ9jfrVevcHV%2BIS8tLi5qMj2lsR6tIh3ExOpvKnpOuPFWtxsNP2kq7gJLc070LzNwWFge%2FCFggpxGpONCpzm4cl2U3S13lSxuuPlO6tJKcEUWqtwtuhbolM5xTt3vyTuNWlvgJMFZgpRvcr1vX5FR0Ud6tz5zMv18tiDZ4Dlomu%2BfDeplWhqEYQzfxs%2F1QQckJdXPY5c5W2j4aBnY8FDfTS0LUcCPCF%2Fo40aK3pjQlmvsJzCQq4TJBjqkAeN1tMZVJllBfgpcFF%2B3404hzNv%2FGAM1m%2BntNpMOecDLgD4fz4gzebi8xwojj1zpjcdjbaCpeWqBaAxnj9Fs0hEXS27fG3L57t2%2BDzI9eER3jISahCxHHDGQCs9W3F%2FtY0TVykIl%2FIGHp6nyHu%2BnaullK9HsNEaT9VcHiDxn3dHtpX260TSHwT5IrEcb0cuwl4%2B80UCQg93Xoou4mHFIBiCl7ALn&X-Amz-Signature=ae1badb37e75e87cee1622c0383f2019f0801548e5b99d320d13be15b058563b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
