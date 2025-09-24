---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QER3S5TG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5Vlf3Nv7pyOcev%2BvCgURIxy%2B%2B%2B0%2FfJfC3JZDr0t2alwIhALg9MRld48rDQJgbpWx7Udni68tFIAQi8Qmbc%2Fa8HpYjKv8DCFwQABoMNjM3NDIzMTgzODA1IgwC5K5TATTo5tZhXFMq3APmZN5iEl5zmJnOTqG377mM3Efd7KfgyrdQbLzXA3EemwnML6WBwhpoeXHRs%2FyBZQ0ATzkiaj5A682%2BcGDutsxswcsTYxN4EOf4t%2BzqGVtZeKTu2dgUqR8gVoa7OTvvRexgXdy6VkJIV1NAERXGWU9mP1dv8herVCf15VN5B6Aouzks%2Fbrbh8Z2GKzBaUbHgci%2BkTHAxkOHgFDNxkHQ7EWqnxuKCppmLF1YVCMD3miIdvzJtBIO3VBIlUSrRQb%2BNCfqqk9KVaXhFKuI27vHdOKBkLu5AgEU1LUSfp2eH6658AnLOKW%2BEmozx5jweU6PiFxdQPTEIsH%2B91ttF9CmEIrPtoEWSnTp1luX7xKBNDjWcBoxCy64Tur3oN9mWN0YAKSX97gVWmIAeKRya%2FovFTGV831SlRgr3xVMrXya9YOPs8S1zMxpP6G2ZaiU32gU9ekECuoLNJL43wmvzP6fev8BUfZQWGUVYnKT%2FTMoBM%2FLhrHnemBWevi%2BsVEEqvDTG0TMNax9fO3hmJcGKfcwzt1Y71hpCuEP19s%2FpfTHAvBXdtWa%2BP51j%2Bd%2FPcaL0WsVBB6%2FBNhjZ22VkZvrozOc6unM77HM3byugmEcASTkflewAl836Sn36XHxHO1VDjDflc%2FGBjqkAUegd7k2cuEtxyiWppBaK0agrZikyryC2LUhzS%2BVpJf5X%2FyEEDQIqpz8k9T5zKL%2FxwZ9B17sxGtEjFjeno1Qw7Ni9Kchd5AhATdFf1Z0fbrkGT7iFe8ca73t%2FR32map9b%2B%2BfeNbm%2FPXD4qpswyPl1GcOMfOo5dBHkU%2FNOKmLQmduC%2FquxybnSWiSApG54ASfqQrkJMDQpjMut724pU547lolxkfN&X-Amz-Signature=f6190280ea96d1b50630b610260658d3d7794a72511ec6001bb83fd0737d5a0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
