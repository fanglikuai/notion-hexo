---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y34O74NI%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEiyGxNvHIwtoXKqvjqKyKz9nj8Z7e6ymB7w7EvBGddKAiBQi6UJ4gVQ4cG4NF2k6T2c72l37ot8NbI%2BW1tk1d%2BA2iqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZZVWLrBAThKiGw80KtwDvpbIIhY%2BQz6Me3Hwglv9lhu2W6tuMQ%2BCXxXEHXkooOwK2I%2F%2Bv8BylS%2BZTGNO8zRpMGaob62vhUSTZR0iaK90C7CfUZ502QEvsmgTz%2FoPn6y4%2B8B1bkrrqIJDDMHviVVOUSbgStpB6o%2Fpa%2BoqLBjFDih8h4mYD%2FRSdbYOBWrCQAgrUbQ4TM08T7eLqBiHrRrrW6jG%2FEYqYSaHWduaZRNXubkc2jl1j2c2VLbcQlGGiIYFVUMPtP%2BtO6vDRyCjFwG%2FJdBalFApO8H9Y4AhH632kJtVeJcTYAQDkEvnYJLaya2aYWP%2Bi%2B5FUd%2Bm4f%2FA4k6Hoyu9TPgreonqzhqeHZtF%2BkkcJZtyPMkMOF7iZmZ3tg3aM%2BllJQW8ntsGql6T52q8qZGBOz%2FdwXm0jXqc%2FYLt8dfRsGHI3OBFCqe5XIjwiB7%2BA7aFc23UpKYKxmCkqrzNXgEXhDVz6mvDu1b1fnnuK6X%2BI7NYDcgpAPAN8a4iXehMQlwdf1ecNDYWdiBUKtWyXojv9o0yCnWjPRYDjAJDiuA5bCIXKhQloMNEUCE8Zl1rQkDbyEAJhPYOcEjJEsBG9nSc9V7TUzfN7GFn%2FQOv%2BffL9VYpbCuglzX2%2FMn%2BpLI9owBb25xUTccvUgkw3szpyAY6pgHpO53PPHTSKyB9d1V0opYnMC31ICaq8GeBx36i5%2F8jBy0r0tA%2F7XIq8xguAKiSNpaeHJJlaiOOhj5n5a0TMeRQ93s7ygk3G2u5PpenzFuw6OcnBBvEFTD1ZgRKWF2sEnWJ3%2FAqm5%2BV9QtNHhHh%2BJu8Tcic4oS9vUIG1TNfRt4r55%2FuGiKLvwROgO2wNK5TWvRzZ07rXCjW0xmvRqtbyt6NM50y4TUg&X-Amz-Signature=3ed4eaba7df0ac9a5a3a0af327a83ff4ae7b0c713961712ee5dd0948bf5fd21e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
