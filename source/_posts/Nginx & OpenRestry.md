---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JEI5GRK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIBTA495SEbqH9fqBudcujxJRr6URpFuKf0UF%2FZimhs%2BBAiBQPpMCx5eTnGQmZ%2F85AmwrCLpTuo47SKt2nCcN5viO%2BCqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDks3Rb6w2Lvf%2BXK8KtwDTvTim8%2Bt4V1oDfn2OlOVYkfKZbIzDpwKqI6zdm8FyWjYyouf8szQRhIsYkFmkC9LoRwRd5HCs951H071mSPGnNdn5TRNk38Tl8fW0pqxLNW9tcc5gUdzpji%2B1G0bAlAWXao2RRhbG1Jcsoydfb0F8ce5eHhYrdA21J0IQJjq7Kz2rTjrIMHD9bHyTi1K78Z9%2FgqKnb81vZm0HXg%2F4cLBuAcm0GrVGXeOszbev9z02BJgqvbfmRVMa6WJiaZTdL7T5Z0n68sW3kivEY8oKML3itXMemnv6RW1%2Bjp5o%2BIlPzfX5ywUZZ6t2qoO0RFJxhxTXmAD7V1Iz4%2Fne4mwKXd3SzFZZ2slKZPbenUpM8TLj%2FElS49n%2FxsP8%2BTRqZ6HAclkPXhtc7OI58gl7AhafvnBecT61%2BAS76Oy3QeJypZO6EvDkr9KcbgMeNTMcwfyR9nj64qyUAw%2BJfIAYX1sWfAKlzunjXnT8LCZbHcHm%2FscSKUiWY6k3yR0xQ3IWd8DPVcs0gkBw9ARhca9waLJKst6aWJVLZj%2FILrQZUbgFnyHvh9YQPlMPJZIs7NLR7%2BGtdPRCKuyUkC8H3CijsjRlAIdKuY1XaLZtjYAkMLnzkjfRcvO%2FsqPTiXo9CHLWbYw7OSgxwY6pgHxeH8ufMC8Qk2eIfShUM6ISco1jsgDYygUWPRsGxQDKB6BV%2F1jV5TEDQG%2B4VQW6HEKAh9VidQ%2FnyHvKJk4XdjKSAeh6l0Pdyi4EIpuQ6QhyyzNANL78t5XC7jGh3aIhyctgyceMjdWoER%2Bof6hynmpxZbBim4G5qMxr4MpTdXrST6ncjMjGLRElH3c%2FwQnW8QgV6KlfR9gogX7jJKqlZS7YJK41e%2BC&X-Amz-Signature=f5f0701a8ec533d581537f2b27d5ab25c72e5cc3359d493805e1341db51ddb18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
