---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DXI353X%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIDOwldS7dI6soJEEVPLBsOOtK9RVzH79ENyQBhB4Z2%2BkAiEA7mWga0EDEnk3jf8ZZPjHY5hi9EiVFrO9wNyZ4HS0iUcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL2TS9mXyWQl9%2Fe6mircA%2Fn5ZRUHWq1D4zSurvtsRj4WZYzqrk%2FH67D9pC3R6mFBQzoLRCNdX47fKoGEoPJ5aznAByGKtG2zWBD1%2BsebJdMgNeSJ9vQhDDbh2m9xehlhWTdzmzqsx591TY3YVnFHXWWMBrxJOxNKYfklPHGFJ4LGRQzRvmx1axM3voaypFXHByl4RdZ%2BZk8u4y3whWOc4jwTi%2BnU4Y3W5%2F6wTmHMwuJtNMEXvHeW%2BUUmMCrMIGFk%2FmRhRIWpETBCRSokhekLgprKqFkUBV%2BDfWeLOXJMmftx%2BsEbmmKb%2BfswLNXNVgHf9v%2F1MRgbuqB8xBHnokYOgZB8WTiizAP0cbgVlF7vLp7eGTC2r6DxfbOif35bgQoWpdjgdVSxLcHmvshi%2FqU7hPzWQnV3EK%2BfccuuCM3uAGgTmFU5jTaLu9qyN%2FucnyQB19%2FSdnU4HxDJ%2F9fA58syA5nw3eTy5rg773z66ETQaDZyutYhe6G0BJM6TIferZN5OET1jKWpnbSoeRs3HR9janpqF4JlaXVa1LGcEvdr%2FJSdQ9k0vRQer%2FFulA3PVILDs%2FFf2ckOyLxSh1NVIKtoeVp7VceqxRtjLr2BhElFoP0FGqL%2FhC2pVS%2BGABV8DKhFLh3U37voh93z8rKJMKiNmMcGOqUBZyiAOU62%2FdvWJryKzcbTvMPHo%2Byink8JZNn2o%2F%2FKOZ90My7EfJtLtCOn926gk4Y08HRsR8wBh4wSXvuVlM09%2FXpGcwcWJv4eBzZUR8R%2B0h4m1hcBq8FrfW3qbEF2ew1Hhqqr9vnvE1e9gF9vbWhQLCWjswGbcO0lk%2Fu9KlIiQ0GwDH61aztS2rV3JYqFJpR1xeSIDdeCqt7%2BM5DFox%2BvrMigAJrW&X-Amz-Signature=7e24dd0d866886204731c0d8e3d4678e37a25ae7a7efe5f07c0d2ee766d33216&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
