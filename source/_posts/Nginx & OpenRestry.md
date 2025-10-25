---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2IF6SEO%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8psOubAN5pc1s6dN3E2G%2Bqu9mhuuuVV5%2FPTWPHgDwbgIgDH04a%2Bu4xy7byRij7c6tuWBJ6fWGJE0v87%2FXjgZ22EIq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDCsvoilSbS3YoG%2B6circA3S56Z%2Bj8n%2F1FS1%2B%2BV4dX83oUMmew9hczxcVkju6ue1%2FuB6DUPZEQ%2BsYpEfkNIy7pKz46zFvQ3awlA9aQqZ7XuSaif%2FGBlnhbbbyBkgq2aseG2GOKD5dgC9kxEjhT14L5K0BmP8R8qVZ70rVhzFcD3EpXLSZGnKXsBSkdQ0E5SN8y5jk8TBF6Pa1S36xiBu2JjNgOe%2FIGKfVlo4zTsrTlo6nUI9eWHIFOUBqHhCm05Ss7ZgaBqNPCFw%2BxRMEW6cl3NG5L2n0ewACPyaKbjV80xKjc%2FGU3iVr%2FSaEBk%2FapUFjsFBJJYoJaFHQxM2E4aZftYkVJTrA2SZjIV4yfPUWxd8Y64emf%2Fw5u88Ksaupbnkq%2F1rdvNQpMaM1hjjzpcyBahjnJJrhS27IvQBFby8J2hf9Hgax9NsiOYFHzNNM42qKs6KfO3yYVxgpcYFfOPZ1Pne3uKTHGg28e6sTJIVV%2BpYNMs2IzHn2jNlyB%2BzFRGMK%2BWVi1hNM6GCw%2B2WPWpGPsr1ATHU%2BoxqPixBtySkMEgKgUhxcHb%2BTUVJULUn27gLuQ%2FAT56v5glzIHrKd5gFOEJuM10LyO4B9QkhVH4%2BUzt9POcpiqWgQpSMG7NuzQLusTIxgNEOi65M51%2FdtMILY8scGOqUB21Rd4qwLxUwCQvbK4KU%2B%2BelvLi8r3P4Qe%2BeJJCStunpb%2F0wl9nc3pA%2B8tuSHME%2FdcX%2FyWi1FgJze50A0sXwrUSDeiVSBFUX01dvcXi6%2Fr7yW48vvTb%2F0QWV7GDJquqX%2Fz6PnnWp3oUs4SFwjMEak5Avxwd6KT9GkEeIgO9tUhohhMi5OY%2F8gbH9jaZRB32lDG4He7vJ8SOozxHoqfF8R43mpEo3h&X-Amz-Signature=35c55bea14744de073490302130a457822d0646d8bb30f9c8c5f1511137ed085&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
