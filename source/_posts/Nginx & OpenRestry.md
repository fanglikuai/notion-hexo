---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUGYHPMT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIBi3XFLOiZhkv8ziLbyC2hC0mTFWQQuKlkxpaCb9SI2UAiEA8zIvKRCFYV7hBQulrq9YIsZrHFs46dwF4WJHWTEFwh8qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMK%2Fa9oHQ%2FChpeaqlircA8XOWG7My4lgON4%2FtYGh0sQnQAsAZvX1ym3LlDkEAEWORadfigFDHm4bJrY7FEmXnjMGGdphLX9rh3wK1zKAcBYqNegCPVhESZ9C%2FaoVqCzGtrNb4ZHPX%2F4jx8ccg6L7ptF7mnVCBtS%2BgFwRYtbw%2BNBGenyAmbAYEX3qNdWrJymAQFBErV%2Fx3DS20yFGIkH3HJfgSMtCDQJuT8gV11jHXEQp9zmCRHmWH9Bkn8H72ItPCkVQOCvon1iBj%2Fl0w29jVq4hj7oUoARE5Gpuipt1%2FGUpPOXMDmeEUU5R43cZEz29XJTvIkL6DlysHRtGKpFO0oLnCTulGv6dWpq2HEn%2BxJNEKN7VJkM9xZeE8hjalpfzm3wP3H15LU7ClJDDDgvxKKzusSI1LqgorFaIQzJZd9zaczakbniYpEKjyitoiEPiOVvaBw81TjU5TQMh0JqR0F7ESy0FcI8Q09vlpzCc8NT%2BEIjU%2FLVta81FIxfV7yMM10licr5ISny5%2Ba1elJU4i6Fdk2zOgEP75NadbIDrNSo7FKKNrCeWrAesiToFnQhiOlKXP4NCmx01lcsFJXkNjsbq24iK5VgOjGU1YSwmoVrcYJvXduDbytg2cYMa4NL%2FPvaX376Nk9%2BBD4f9MKXZ2McGOqUB0qQ6Z3nEfhnSRlCmWNgvGBjJq%2FCOOUFrHKpRkBJZFlIL1yfAPeF%2FC4N%2BpsHkPuEZagY7N0snF2fIyYvwz8PuPXprup433N2Lejhz7mllAaH5p2tiRV9nt%2BtYtIqYnOIbZmRK07HNYRvKXhBz3C4VIbnZQViopzV9qLfTyXZjc0XkTnh0oNBOeoeX3pZ8vkccFxHhTEn%2FSAyhvac31pUH04mBtr%2Br&X-Amz-Signature=3bfce2b5de8ded50c8bd9a95a3984c10c0ebdcdea84fadeb70a1799b821ff210&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
