---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCHIXZFZ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T040052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICCgdDMmXewBN%2FnJOrPMjh4lVl%2FSYBcr7Sjy8RyOW1%2BaAiEA2Q5xSbMgYNEhCwXYUJcE2hR6yo7fzluX1IG6Ex%2FESRkq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDCrkTzq9ZTC0LAb3RyrcA8y9YyQraSwEBUW7xM1%2F5CTrGQC4dl303%2BKQRC1CWf76GgjasutuAPrH0OYv3Yb2HdNCoqXWFUdvLkyb9mil0%2BTcnEvYzCYoY1YBBA8vyK8yzL0lkR5YcA6Sua0SsBhJ9i%2FzUvH9iqbFvjuBaAhp6H29%2FocoXAg5hKUlzJMd0YQRzRjyBmDc%2BhG2UAZiACmrrDj2bpnlrTfLksVziQrsCqUkFOJZZllyyCf61ZOZZ1%2FvfaiMxrIF%2BDYl6ylKzjnhMZyeaqTvC4fQkkuLeYJjOHirkLGwZg%2Fo0Kh3S1u0Aty0OhK5pSxfhCREYR22MtozakNm7Xvb2aYXyMDLoh8v2ulCKCK0sGEdYHWbL2lXkn6%2FBhtmFFl0gTgtFpzlA4kNFJ31WT%2B8TjJ5Ly2vjqXsdMZqSKhtJjuJA1EFriQeofi2y4CdiVU%2B6RS2RRiTCXemv%2FulCYqCBWIejzba%2BcgylhPyePrKrtSXYmLeQwjladMyU5JTgRghmFYV7yo2GC9CGw4M8UWCkhwDq%2BaAMXQFDZKM8uAC92ZTcjnDrP4n1lztE6009pyriVzVSermc74CP6M3aK6eSJv%2BMWKuISamGTo5u3RQ4lexPSpF7pdGPABhPQOpWwgfQwEcZg3LMKLlpcgGOqUB1OIv8a3HFz2cvcoxJofKZ1QYHJARkYS3%2F4uYA%2F5FEoyr28wJcE7Pk46rYaVYbl%2BkLx3ZlfvDcD8ZQw4HEuB6AJaQI4PWEOFvp13zwvslXUih2fbpiS0a5PFMA68LuS3nBQQYwqa%2Fti2et46U7Xh%2FyZ5qoNfkiVkj%2FUymqaBjyniFEWXMq3t%2BiXgUXcdehK999Quc8qUNHj8%2B%2BldMbhGPY1vrHMud&X-Amz-Signature=1a2346750865a67cf434f65cca6f4aabb7d555ee2738fff3272ff67af93be6d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
