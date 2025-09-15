---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VM4P4LWZ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090819Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmj41ExhqSr1BgnyUIWRfqxDMG7fWB63XMpF%2BNLyV5GAiEAqnmc%2BpB5cJnbsMfMZPyNjRCT%2Bi90lTnJEwiGhOq7ij4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKyL1rvodNrB9niL2CrcA2uXjnbrsKwqyPKdW07JLM9Wpui3jZ4zUGQYohnIfpOAZsOyA%2F5yc8WrlFzhw8x99oOBcYLCNkYY0b%2FJ0Ycxvxfo1pJAVK3XBAHNX5A9vkEinJf%2BzqPgQbUL5c2CzvPOwRL6g2dNh68H%2BESlUnbKxriqpNo6MwBEecN0Yi55wOcH9kNKCbpkOWkCvB%2FWm1ln21oOy%2BWSO%2FAqmexo6%2BNB0f8p019e4V9aZOV7U1wBLhY%2BizFSxwOd7h0jdCCgE9VVNjunRb26QAIvc9J%2Fh1pNmE5%2FiCi0q1RVceT6LNXjSc9DP1omf0nvQcgUhtjJGHlHauEkODqPeFFcRURfI1LSn5FRUP1XK1CFZmidyUYo9wT6EA0F7vYZTKLvArIdSqsCvFTD5EPQ5g6WKWfynzA3cQ6jJKswYG2AkMGSEK1vXmzXvlKROV%2BAqJNmTSRJXtGJfhrS5V78ywvJisEJq8f23tPa63%2FuHkvPQwYj14FBwoSVMk5IGNcB4V4C8haWYBSdDm6GgsB15%2F4dd9CPnx64b7IKpJCctDQgYGmyiw0nhjDvsu1JY6z73M3Rm7QtmmT%2Fapar4TX%2BzVASLSOlHfAgG7FliXHc8hZR%2FLLLs6wH%2FkodHhqyN%2FSbqtkqtCVkMJGTn8YGOqUBXUm20HEiXdSKLviL6K2Pv4us3zuPuTS8hsu0lJMTnwre4gH9u0f8eQFwrw3o3DxiioXgG6uuLZSw0eiQNUSXoaX45n9l0kRHHI%2BWFWkicYxuQSNBiPjQAAWu%2B65Ml6z8v3WMKiOjBTLwKvuO7l3upok9oBaXyRAqDu863Dy3nKtKd1VRT7OnkTCjsndd3c%2BhwwbjqQ5peOS2PpPdyjoV0E8LkZEp&X-Amz-Signature=e47d79075822342815a120f6d0ec2221ad0f7beb7d5ea344764c0464c526053b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
