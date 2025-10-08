---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP3ATWCO%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIQCTsCM7BEjnxuWzqa1AqcnBvmZtqMG6jKZGQpq7V6u5bwIgIoFGPFEtHoi6iUokaogTJt%2Fdy2QOT7Oeit6euxjYyZIqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL0UulBf62i6zG44SircA9r4upvhJhgbLXsp7jB3madGzVNTT9EdNf9U1jjJTqOaLazFawSsGANZnJtJrgPvL3DQFJJlKvvD2hlNmSCGe3Ohz3lJ49k1PVmyrBGzvv6UjGzvu5uWICeHEuZmW%2FNcxrEvE6HFPsABPR%2BOfsZ3WyHQqaMxj4RAaCUZcxVIyAP%2FE%2BdIysDAmxPuWdG%2Ftc2S%2FfJrruD2VhWrQVld6b3%2B0DHhnzrPKdTI3O8Bz%2FdPKipcxioSWEBb6JpeSKwGwMRuC%2FFLWZzpXu1QexQHRqEDotubTNs2H5KKdGb2jpDsmVEpobhAFwXdKaGmr1HaLw0cdwooVEY%2FVQKrzjipzuye8ahZDRPlCRElwQxGSyqZK0PTo4Q31UfRazsV9CuJ12c%2FMz6OMaRKPvzXO0IU5GSgerHyJidRCjYBzBG5WWn5lrlaZbywXeP%2FEKtoltLalC6qkjV0v7OIaYWkoM4VmxGWuHfYczdLyDruk9N4PPC0TDP2Mwt5cBpEjxgb3xxmqaQNyUsZGGY5i%2B48ULg%2FAuz7eBo6Q56ellu%2B3XricyPhLKGxvllmTRtn%2FO%2BZX476jFXIN%2Fno9%2FCQpYJVTjkIngYmGl1VHHCD6warzTkCK1Tvpo5igtG1%2BAki1ZhGLkHXMP6OmccGOqUBYxi6FCD7jl57FUJS9P1RYX6k9ITEgtkEXcxT0525ZCxJy2PLQx7QcOptBkXnzFdtG775h%2FWAo3mD86ttHgXRxJ2V0U9Tp5ZJNylM5KB6y3Lb%2B4TXC2tHiltDq6xk8C1uy4XO3jWbKpTwx3LRHjcADZOyLQ3yTif8SrMCLPNL6Unm485U8%2F0OShvkg88asrCReW%2BFVBpAmE4JInCArPC6EBy8i3j%2B&X-Amz-Signature=107af8c2ab06c2cc4110ef8fa375ba60b608b2301be0dcc2763e87d41e62e604&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
