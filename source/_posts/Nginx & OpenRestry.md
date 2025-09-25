---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4T47E2Y%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDHTRdVyRUP8RL%2BziDHj05dcM68qUR8UUEaYlzczI0baAIgOABzNWJrLb%2Bc6J3WJ8TI6ya%2BpsWiCB1owN%2F12f2vVygq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDDsUtCJE6HtUcjfAfCrcA%2FPlOHLAg071sjzqOPazZBPVtdoCeb7ywsQQrXRod5pzTD98SJWioPbd8GxrQTkvEFTjdNZHuTTEndBpEI2pa0cUHqIVaS7dMr%2B0MSd0cE9Qygl1h0HgyM%2BCKCGRXR00n%2F0hS8tiuP2viQuhtYn24fODR0SlxyPRKUX%2FNvRhm1l59epjaYrGyTUj%2Byo1Va%2BDj2d7fPMtwGG1w5y36TvVZT8yaOmvZDQQzVGtKY4JI2n3ShNgwfqG83Xk56MQKdJUKJOCfwjwckXvKcZw8wWMUaz4MXrClfSYPHljZ5f7JcirdoHFOH02XmOLpCNIDylcd5GB3Bu189uRQhUpkHouCTcck1fQ0DS1WIgxD9q0B2TZ2g%2B9I1zQf9n2IffZzzjMBIc%2BKPI4kYf0RRu6ugjGJRBSsqFbas32WVu39O%2BcL%2Bt7hABA6wNM9ixC3QZNQEv599SN981UAkPhsS7F7l%2FAlzseYZRZ3EKukGnYiWu%2BAddxIaIBpJU7vhbeBHllXgwVaHx8azIIUTOVaa4db5ZMrY6fP1HLYwVGKdThyWCbmv3764H96paY2zfpoJHhCDV%2FQyv5L%2F5mZNJPHVnIchJzEJL%2FIwgjkctgjE%2Fd1qpFI078Mc8HvCElJtVKCLX7MLq91MYGOqUBSeNyg%2BsUTGhHWjAp6OfJtW0%2BZIaFIftwD1RRRMvDCVqcMzXq2iwPHIQ5kR%2Fy%2FioDhdWOO%2B68ht5wj5EO01OH6wJXlDM7eKRRJpsIIAoUHt9oxaF%2FhBhnniOe4iqFLZ2NytktlyIYOCpOkhATi9F9DFwXw635bwijK5zac0bUrLz1gfz8q64L%2FHj4CPdEkRrUK7A5iQK3ozF7MQV7R0QJ%2FTYteYR3&X-Amz-Signature=2777dab68120faf9d14c0951cdb60f74ecad37f6624cbc8225fc7d6ca0f4fb63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
