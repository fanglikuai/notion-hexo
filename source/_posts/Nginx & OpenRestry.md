---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWERFRW2%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICX6t4b0%2FFDbdh%2BmVpCsMdwJPs2EIi0YbN32j6LJVnYPAiBxWO2MpHX5iIAPR7Jip6aSfo0yrQR%2BiNwtndCZ%2Fc45nCr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMI62%2BJ6cox4jng8vxKtwDslWrWtQQdvHQrjvmSxcDMC62L7JWrGEi0%2Fwo%2BLmepUHhgNiA1o%2BBBQP1Yi3sVlhznKtGZB9oavPZpUzCsjxW8zJGdp0p%2F6uX6zOUSpL2HVnAY1t%2Ffl%2FuVn7IkOVP9JUOStIZIbIijG6gYU0MT3X2kjZryXOUQGKjIFqZIMxyXVB46SbSzgJm%2BgXSjzWScXmMItibZ2KU7hxhFWGKDpljEErsIN%2F2Rs6D1Ptmp%2BTDbFG62hLR%2FM5XL4th1FWQ%2BsZqOTW%2F7KaimBeoXnHHC5NVoe2l%2BG6943cL8e66qhir6%2BjOURwNWjS4VSJyCkmGV3uKIfJNJKRbV%2FyslzJorQ%2BM9K6CEG7Qz%2BWmpTNqZyNJFRLmyfJn2WIOnlYBWX6P2Mr9QkqR4UGegPHWkh67Zo3g5vm1mspDG%2FMGS5GT4TVxDdwtrlQ%2BzZxLjVwBtPvsL9BLcD71GhsaHq8VprjZTLoH457xFwo63eW%2FhLjSYevg7bGV3SRnNgEtAgYXXw4uwIxnBBiv19W9HGIu2%2F2miZWdzr6%2BJE5nNw%2BAsOffFXfUGas%2BWxa824y5KwU%2BTP5b%2FFeyE1K0sl7wqLmrqvLZr%2BK3yQbS5X682VIq4SgQJ620H91FOj6L%2BK6BnriEFV4w2PjPxgY6pgERkknxieXicX5f1k0n%2BAQ3wmmE%2BNUKiFZwRRdRwiWjF6PlfTCQdBW3KWoZrFXDhC0quh4AhfXQUCpvCcFujvhqJx3usyW2%2FWOBwd9o2qLJNVH0hv8lIrxB8KdkR%2BCwlz%2F%2BDZclRomN%2BiJD%2Fg7fTz5qKn5NF%2FhRHLHCwnZN08WS30NN1UdaSlWnB5%2FIfwruUzIIXPSUhOqaFHk%2B8CdclmX8xDrtcrbp&X-Amz-Signature=4bb7cf4342de7c5fb93f370bdb477d7de49bdf4b6d42b8adbadf9e6dc911040d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
