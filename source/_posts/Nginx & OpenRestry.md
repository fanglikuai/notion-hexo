---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYAJWVYD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDKiYR7CWFBuHGgCs6%2BsxdG65%2FkyYjqHY8u619o3wWPZAiAPCoBdnXQTw3uPW6gHYU61sl9%2FVASsh4HJvgV4mHPnFSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMu656bkVd3UzwfhffKtwDqVVW5Oz1ueFiC4Tkj4HdbARgLL4D6Fd%2F3%2F1n%2BK6RJYo0m%2FGpy89DZgCHegUk2Q9QhzSNpcJq9xHE%2Bvj7rHx0lkbXMUeJaaNW0Cu%2BgIkNUm7zoD0rc6VRh%2FMKPfJM8CxBfILHoagik18KKij8myn2kgm1ZdvrVY2S5oLBRQXPmX6HORtz3NEWmuOUHGJdz2p9zlgrhfadtddwtzHgSTOHu5ts9k%2FcoXKU%2BfT9un1zDSJdoPGA7%2BWHz%2BiEQS45iUrhnSbnPUfoeLw6yjpQ2tKUichjIknl6xf5Ecvp5vQyqKFMiC9frrql4jC1G74AAnDlm%2Bd1ezmFHmrACur98Zld7agOZ%2BhVf9DM6XajS5DyU5xhNiSteYV%2FDQvNe6qzYkJokT2fRNCH49nCdl1rr16vvOxsQsXC3Gwe%2BrrSVq5kBqnRIdR%2F3FVS4tLU5IsMj8WMGLmZAvH3KZFdy%2BTQaeHxl%2FpR4hK6IO5DEFu0PU1CG1exVr0d86hr7BanMc2oIBpWP%2BEX7jxHL%2FLkXXXz2q1TnFzMHMeZ2pDm%2BK%2B%2BoeJiN4D6hZrmVfSUnzm38Xm8E5VqPWScGlzzMEa61XU%2BH%2FA047XEwaLetMF6W%2FYhgnvb1X7FTpPKxLCKu7KRLiowiPLVyAY6pgGVQEfn744PMy%2FIz%2B9hQXHFiJCUVc31khTM3lmozhw9W2qqQJWW6IXqXYBcJqkERxF7UUzjp3ZA1gyzXml51A5qwBPIhgBYx2IadDByZNqo8jKJjaBqzH9Q2jxU46GRQlSFMV%2FS1cFqF7K5eloT7ReNCCWGUOhoT2n6meks6zRsx0cZDZnnYoRayRc%2FGMHoOcGP6TRQE2paX5fnQXB8jUbHF8EKQXZH&X-Amz-Signature=793f2fc521c922c36c5cd983fc4fb936e0211384ea4393732f5d7a41a14f82c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
