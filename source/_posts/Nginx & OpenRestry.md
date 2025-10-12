---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPR3YWJD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAURoBQpGw947Ch%2FDIoy%2FOWf6dCAm94Ntze4aIs5oHk9AiEAgKU0tsT0aO%2FUO5WeoEx6%2FwtZSfJLxgJGRvz3uSRvj5Yq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDMjZHQH49ORtRJG8VyrcA7cLEURwiHa2O%2FADBRAWVhyFqNPg25lBCKA5QPvQZX1Iw%2Fm4IAM%2BJWOu4ECekzBkMdx8hLyXGIzy2p3wmemWjO8dFTY%2FBtA3cI9%2BaA0rpsXtsTMegNUqzz4wXwQCWmwKNc%2FaqKuXsZ7FITJ%2BJpYlYn7z8Tn8HWUr4FfXZnLNsOE2jAb%2FSaSI08BIl7YOO7NXEEeJsmElkEbYzru%2FZkrpkSNDWR940jXDMje3ByyoMcrJ3ql8WUVgXrLBkq5WtMb6Ps%2FFMsyk7ghHGDC98GrBALRWkc1sS%2BELNIwzmRsjlaKWbQJmYfkkJJCkl0pkg3y1JoWs5Yb%2FEIa%2F25%2BQJ9ksS8QvmFdYaAp2jlqZvhc18cIflwJh4elXGBVjrOGyO8kVbdSgobS1sM6jKwRnJOYs%2BqLyXkPSO97Xh5VUGgY8IaxgEZJ7rhu8l%2BtMK5C878pUcOmWej%2Fnydt9PaiXuPUxoV78WUfq%2B3rf%2F6fmwJe4lQtGkq6hp9EOuW6UjVOYyrDMQkBDfPrpbnbnBVu02D4YfeBWzYCyiaTJj4W6MCUGYdpUA3Nj9Qg%2BJJQ5JXjJX818Ll%2FBNoNAViYM%2BNGxP33tszcwSv3IlJ2USWb3%2F%2FvKVeYc3WV6%2FLdXqX6O6sW1MP%2Fqr8cGOqUBFOrn3Z7G1jCdC3fE5NiAPUB5AK4JA8G398UEXVxNxlIe90uiimf36HBr%2BlY%2BxdvEmxtt%2Bm02TGoa2wB8IlQYId0fOQXOnx%2F7etV%2Bld9uWnfKONcsnSC%2BkKU4q98tzT9eOPme2dCDB1LCCA9zlMHpk7wXxbjyiyX4B0eOtWg5aYqujdGA0%2FDFBUm4%2F1f8dNoufDoQ%2FKTDaMsfDASYb1yedNgHKwiH&X-Amz-Signature=f48c9e1ae410173defbfa2eff2db4cd4aab5a9e9bed3cb7db83c2523fd0f9315&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
