---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PYMLA6P%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC67Vp04kEW9WeXSMC0n1QMms85%2FI9dbEVu2Tkt7bygfAIgUiJRXtD0t9SZmvDZB5799lMd3oLDDbGnsaoogqICOg8q%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDOFKNaC7quVx%2BV3d%2FSrcA8gOzHUBuEJXr3OV79NyB44c4C%2FEAZ7MdqHDC%2BPSenYpYxwPk2pjrrQnsnVPrARVhc%2Fu7totzvhzRLM3WOGAsgy82M3jo9CdADFOiyfa6rufsLNZTmUdJ07QLrY4p3sw9X41UzXiwYwLs8CQFc9iAn%2F62qUgzuhJbBwpqHX9jORIrrcqzYEPaXaSCbSl2CDCUFwFLXQU7sdjE5zJEE60pN%2BlGbgK7Zd5n2HUVJvXXZ9lB5NaynQagCtqlU49%2F2jWNvS%2B1%2BmUB7b98bHuSKu4Gp1QwXuSqx4DB2n7%2B5SsSRw5HDZRVzcIXCwQ71SnBVcBpL3pYqUt6D3%2B1Vd6BcGh6l6WdNKFhfN8v5BqImVkzVYnfgpp4ueAJ8Vjj3LyrGf70JayHoaxT7iF%2FovUfvS8gebiiwNQaWYVyojTWcWyzBZwwcY5xvJJfifjeHNp6lA0pJDemy7c%2BgbYL8fVhPrm6LUsLnIx%2Bvg4R7%2BIxyrlfHxLwyzmsk8w0m9kE4yCDxF0xOnHp5P3zHIwkkkws8txQW%2FIDVsI7itmUfKmqCZYQNoH8TMM%2FIzL%2BZa%2Bkq3MvOcf2OkOgvkqlh93BusaPvO4BV5G5O1KRNM%2FmM3b2shIlZmC%2FqWm2ZKgDFc7e8hFMI392sgGOqUBNODUjCnyJvE8iBIt9NdhpHoVh7GbzqY85g%2FHhe8x2rUeMpj9Uf7Dl3GpVaxNHt8ck%2BqCRFm6rC3uEXTLrxLRyH8mNCZkN0BC5Tssntyc6UpByWgP0cpHDwiDIQ%2FcoMeQeljbb2MkNv42lB9tML0Uk6v5ovmsyd8vM1LtoVDeX7zqcMCs9uInVbzWnSk5fdws3g6kPOYbyaJCfOEJRqr1gYzXKT9V&X-Amz-Signature=cd5111768a72f3b1bf1a10eb58094216851b01f2f605d0ea824fb0f6d4c7190c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
