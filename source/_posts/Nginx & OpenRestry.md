---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AIE2A6A%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDR%2Bi5MLWzKhPeD4gg37v50W%2B7fKFencrYqX3fgm9QkAiEAgfjGODgHgpQVWeyDpRU4BkA1kFLZ%2BSrK4phblf%2BiwRkq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDA5FuHDj%2BnGDKbSjuSrcA0Rqy0MwJh2zsNYQjjoDbYB1SBBOJ7NxTlY%2FF%2FV%2BQDfRIHR5I%2FjUKxEI8T1jo%2BbtmDBNz8xB2wkS6U8E052Y4QPorgu%2F6ZwZhs%2Bje2dPisuP7TmWcyje%2F2u4u99Cd6DXecaA1MuoUfw55R3aw43yp7sjkffUE4psITetfwoeYguFO0Y68048dvokZ4IJ9KcMbsBh8JTjyPRhhaQMrGJFOl%2BnTTvnnxaJR4Drx%2BCkPe6er5gclM6UrirBavPeY4OB3kS0lEGOOMRyQJZ8hy6MqKbG%2FZpNn1H9hwUCDCVR%2FJjMMsUvsjeCqBD8rvgbxWNGay%2Fu3giJq%2BB0jYJF8AwNUeJTDKYWfX4P3ZRewHsSdbzDEiTDe0Q%2BJYQg7GKa10rDt2JM6N4zorSZvEVaTzmzffeIPvc6NebBrud5RqiocDCOEooZD5hGJl6QFq9sdUCP4e%2FxwjAeO3FcoZiDoGa82KpJzHpRvHnZTsLDUkzB5aQEqI9UDg4dyeSt%2BdFJiBX4PLXnUFVfu%2BkN%2BNbFT%2FxVt1vhPSaJduTQWCg1sIOg0hPEm7Q6Ool28IqDYHlS9Ow7zLhkGA1cEGPhmD8m1Lj2TDntP3L5lFzNHyxyAtwkws8GmwiYOCZAEhi%2BeorFMKON%2FcYGOqUBqQ9Yol7lCF2HHW1p9VGD4SAtbrey6OG5%2ByQntaaLd86fcjpubNmykyXFLsyjlLPI3dZmdTDyT%2FStsuMGVJhzLPHfBFb6uNVNmrjbjiyLGP1QEH%2Fg2zFZnvcPZ%2Fib2ohEqmeflIQ3hhv6dcX8COJfgQ%2FKup19T6igXv8onrVdGBWBPZMsTIcQ0ZwMQpao6mxoqSaJRHQQPMNaMm40JI1%2Fn56A2tcH&X-Amz-Signature=e140bef0bacd8f9c3612f829715ae5d1f4065425bf65301033eef993389fb5ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
