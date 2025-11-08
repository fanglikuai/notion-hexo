---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UP4ROHYB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDO2PrWiq8cMe7rE5YiUOWU%2FyGejWDhKIuVG3F4DZo6EgIhAKpx6o1Ap4748%2BUkC2hWx1CdQMYOVVP7t2hFdMvijznsKogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwQuA2IqacGAYIx4iQq3AN2xKoOuVH3eIFfBd1l0vre%2FKxa2vPrQTlbPR1ItXEyAZsPFDu11BZBpqKgvIlcHSPZm%2BdSwKOb4UI4I56eCE1Z%2BcuWB460LSzEa8LlNPx6hDF7zF08ZgpCf7QuSUMK8gni%2Fr5BObXKLudw95qzFKQEXQxaN4N9%2Fkf6rDjjjPNmJiJh2uncDcZXJ49bm%2Fq8owm4qz4LT0THACJbs2%2F1JHjDgDAWKUQJd3sfAgCHtTZSH7fFZKWHCfOAmqnRzgOfzSVjOcPkuDtr2Q%2BqbGZzhb8rlj1myQ%2B%2FVJP2POrEmeOuPZd7WSaBzOT1nQWqIfi2MHLgebznyJFWpmAbyFSE99YdMD4adnrnmPIh8sGgZF7dAkny3tKFqycyzwHp1bRvqe7Sp1Ap6I6Hun%2BanMtpb40qIHzFyXiVW9woih9ruT56aMJZbjnhEmacNrPJ28zeRM2aR0yq69YkA7rWN1XsIWWctZu3GJROQTeeYotoSkldMPJ2uE9Ht6VpxjyR0YOuIVASzE0GVudcuOG1LjDAc%2B1feR9MfZ2TmCnPvDhBb0oiH1sbFgIkS0ejQG2aXUMsRjHeMzU2DwS%2Bc0806qSfrQh381Hl6lVPG9ccbWtC38hl9j0bHLre4%2F33niAZDjCSmr7IBjqkAWG1I9LXciKqlFHpt1Q9%2FXapRkXrDGcjnNhx%2F%2B3MegMjZqKBaEcaGQt5yrwL0IZ8h%2FB7z%2BEJEzRm6uOoQDDlzq4t%2FVuVKEWCqwg9LliRwI4CMRCS73ZVa43ob2Xelwk9nPhicy%2Fyl%2F5yNndaEDjhU7xrtMAW6smkS%2BMXA%2F46Vsie8aWm1lUijNtr3M%2BGsW%2FknKcRGNLSMoI716USkX4hyipLBcOt&X-Amz-Signature=d9cf1c822bc6c6f39928669999443f6f80cb85254ab501fef4c5e3a38bd9298d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
