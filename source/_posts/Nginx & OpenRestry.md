---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCLC3IIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCYbF3O8zicU8kicQ612PjeY1JGwxURSXT%2B%2B%2Bbphd8kagIgDhfhU%2FmFjgRyr44%2FoZ%2BzSqNaF5dMmI5ykk1SdIlUptUqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNDmuLROCAS6YFJsBSrcA%2FvSdw1GmcdyzK2kX0DJ9bi%2FMl6%2B%2B7yTZVudDFrFPfFy8bMXnDncFBpqFuXZKKtri5bJRqZ0Yqq8WrMb2c9hAldRbuiRkc62ykfpE4CBRJQfZt%2FEo0bTW0IthH27RErxLjJqVxNHvjWxKwRnJ0Rd3EvtXg5vvd51F3zLsP5%2F%2Fm7LmHxEqT3IM18DBPdQEekOPx6JJwOICRG9HK1RwX5qvS8DJoUM7dwkpeI0dTqAGIJRX3N7VVM18my4tvIEQxi2YwGJ5lWM%2BCTSBDEE1qSr20d8ZO52BwWCVDKtuJm7p1W4RfEofkXpQ%2F0Hn8wsJJkKOKno%2BqqtOjkjkk%2Bqyiou1rTXfADwgxD2xnCKDtkvmPFMqPLVw2Wql57uF2OT5W%2FNQ6b9l0XQGboAQKWC%2B0SuGYNUKXJQBCbBYQfChZXwP7oEfRYrRFbh6vr%2FglHYb74%2FvItC5CAahCKN9PguyY9KWW01YrR%2Bw7Irf2khHIxICwvuUhbnHGU5me2%2BaDp0klTrOig5VD4hdVgv%2FICHzmu1HrzYSb05h1M9ACbdpd3fiy0uwuBq7Wver%2FS4PqNGyDnJNHAaHvhMQnko%2Fa9OhnC8m06xxLcpD2a%2BpkgnR4NDYey0faVlBUJrcnMan2sKMM2koMkGOqUBxGy1UwcrVNDAHmZ2I6mxzO9CrfoIJxnTCRYBgD751DRKaubHRTymFgk2vSr%2BzHammKPSj2e9hXJsBOpgkh9tYlbbpZwg4PRGbFztET6B86z9FJ2Plyu0I5Y4hfgK2H%2BXPSUYZE4gl3yYJHN%2F5S93tX7r%2FTR0Doz5zjxgbmVNsB1kUU6BIiid77aV4p2LOjLY8%2FZTA2BvAQyRQpi78GwsC39fH7lS&X-Amz-Signature=ffa43646acfd3782c331ddb70c1a7af45bac7d91e5cdb6f96392778a48c80289&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
