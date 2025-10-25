---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLPEH66L%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T070106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDprYvx59Vj7b0gaZMq4ENc3IvtO8LZBucHVU4brKhihAiEA9UaN3uvYriIuyBDv4xyQBSqABWOv9Wd8RVSjHwpEfdgq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDArfzUXnrc5WTgUlRSrcA3t0d06EDwuOTCscTRfdeY4D2WkAb5whjOH%2FlXQ%2B8ydtKoF7nmtjmBdlvudVczA5g%2FM%2FnaGMeLjukpaPx6HWy01Sp2yrmk8%2BDIzaatCVpg%2BvvyGtJKE1OoUzqXAnHfX3MsHZlU3kRRT%2BCTierY4%2FNds98BVnvvoImc%2B0hOxgl4ImYbJtWYhqvmUxpyNGbIOIXDG4CKLfeWj7BS%2FN8R2TXcFpD5n4IW6NAEKJoZKPIfLXoX2CTep%2By1QiDoOSXHf2bSIxoOEnZKlA8lJgv6s5xnquLOgAi4rj6vJXtP8pQNKEresUwtTpfdP1PhwcFOah7a7nbhHwA%2FMXjj0dvysYlgnqqMp0XHGWtaK7iZRLtwo%2BU54Dv5obxNJl4AeLcNlODRztrjRALfwWV%2BMtD54rFbQyuK2PvLKzlNuI2ZQNbaUCDfvCJE5t7qaxba%2FQTlvla1xttEISzIwQWzSIEI8IFvuiXLj6o1rCgnTvRJ5wh46DS8FXbDzi7N9rk7q2fYO7pk4FcmYBJTiAPVwBNTv0b7YzE5LG%2FV4Q98PWOWdLM6p3SJC4ouTDo5evMHiHs5mf6tiVNJukwFQiIlXTLOylKR93PtvGKcArEUyiEkFujveEXyeDAi2rlub56Sv3MPvq8ccGOqUBleANX2a%2FHTqWg4Qe9sVRKMLFYisMzyWpnE%2Faf8PLQ6RbhhQxV5VUuV%2FOlNKr%2FI9%2F8wnf4f5kpbw9r%2FdQ0FCBPENBfHEZEk2bAfW5nDkQz4jjM%2B3SO%2B8VlG3YCdziYhzxIOYZg8sio94ACzRWH8uSVngGL%2BE3373uNvOK1Ft7c4lv7OPO6et%2FgH%2BkbqvBXajR6pRpW9k32itrz9TzJ%2FFkoOmf1WLa&X-Amz-Signature=9514cf52ec12d47dd7a87f5042b419a6f2fe0f09a3d436999266f92892ebd260&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
