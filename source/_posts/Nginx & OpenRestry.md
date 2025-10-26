---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI4HY4B3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICWuks8%2B63%2Bs4ZMMd93qGMdQAVBlA5Az531mGeg3wUi7AiEAk96vETE7%2BZAsqouef148Ux6d3aMw4pyI%2Ff0Zxgt295UqiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHR%2BX1X%2BMEDdfI4pICrcA4Au3r1Si%2FSUhETG8Lq8NiNTLp612jXuGsL47Ec%2F21PMb%2F%2BQZ5chE0JzVK0j2n0%2FUetMbCBBPXpzwj77OZ5P2q7m2VDjxNWcO7Vj9jnmAxOy3U7iAJtidqcbhjpQXJx%2BW81001Nl1QD2Sky76lWkFBikLkqrAc7KJ6E44JGnuP8TgqyTVD6GZXDPn0mQiU6PPoFuAJvZK21fslp0JsrUkQFH7t5RZZyabA9vZUiu3uLYD1YTLKIQ2xKlDMoIobbSFYnd2K1G%2BBF7yH%2FwHYLrM8SpzmrmQs6oWDjFCyLS640SpVC9Tu12MZIwwRFha7jTs4M1LJ%2BdBbDMqU%2FdnHepdnnW%2BA4qWxT1i8PHVXeqv39Fv7OZEqHUyrmmSaYkICmuoXCVeUGKJ59wko7Lh5u8QDSugKESFqMxtNq74iLktThs7g5mhfoFJCzA149TWH8r2iX6TrCe0iMzqzWebSnTjDL56eSBtlUimctw3jGoeCwA16MOn%2FqxaUgdyv8YPU2X66Ffgf01rJ3vYRYQabkcM0danHF4XGiEKSBteSByxgI3PWa6DCARtqcXMH5TgltxMEqL7r%2FU0E8Dn%2FkOive3ifBiQKpz3VopgHdKFn3ntqc7lRsdN32Mju94n2%2FwMJrO%2BccGOqUB2jFOWZOIStCaOhOs8BsQR%2FYVjW1chLtjLu8ttHmqiGi7HX83eWq9GuhrWCFX2bxlxc315KnP0U0FXzlOiCKzKyC%2F3ukyVNqVcOL6qibR90zsVRwtadqkep06OxyPm459uapDLos8vGFwcitGns9UZmBUuFvAjV7R9xxYqc81VU2zBa9onrAzxIsi5US7XlATZtk5jEcYO1AJqLX8StFjoop%2B%2BPe%2F&X-Amz-Signature=399eed1133134e512a4f532cb3ac16160f2d643a7e4552ecebf106b5c2292018&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
