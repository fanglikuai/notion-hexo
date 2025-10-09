---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI2PWPH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCICw3OpBZsXPhmUsyLsXODr2F9bNxTIMVwvxLpYHbpT5sAiBvP23lcOsFvQ2TV3A%2BiR6UnsDDOR4RfjQYxz%2F%2BxofKqSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7jWrb8yVL2C4z%2BL5KtwDPgN4Y2PM1xCRJVNbN8t0BvoHSCxVmT%2FEQUWf%2FDPRPI1xSzez2rhf59%2Bfhs3sUIb2o58KWUhfOJuxVmfk06sn5qQ56z9UhneApdpVlaqawIbLW1skOmNqo99TrhyYeUCnkKHAqYBDfHfxpiqg5jVhSzKi5PCVDTYyFYlAX6TKvdyGgMUGCtVe2krFmr9661TPjY8Q%2BhuBV0aFqz5YwdrjYq0Vl1hsPtaPF7TwHtPUM%2BFxenN6h5fRVRJYh3L%2FSLMqiejTIoHLpHEmGy7radmlaeEqkmuTLKZ42%2FOaOmlO3RwOyO8vC6pGgdf85cnMwuOS3m4pvy%2F1LRfKDpkGOnEsCAoJHNDnZfZ4lWzIcyXHMJlRVyUMVPi6mdXPUTQki%2BStDPiHDwV%2BhlQcGKhWQWnL7M8Xl390WT6ZKwkOyHlBlpZ2%2B%2Bf133b%2BDgd3bQrl8YhH2p%2Bf5bGEY5KXyeFo1Dk61TvF1ycgTsIaRcdKk2H1WHpuKTN0ZQshdKkz7Lr1rSFYiTTD29H4bYJ4lT5O0P77tjWLsO3I3nzac11kH9XvUBFn5YMd9sgHE6z%2FQ%2F2jz3yTYaYPZviM2Qn%2FBAqb5e%2B%2FHWo3JOtRTmOgmgK%2BGVcbrFM%2BuTrUoTtP%2Fg7iSmgwu6SgxwY6pgFtolR3LqGCwm4FdVwkll6XpqzyDDeq9ppuLZuKb4TQo%2Fi5Efr4ngaGBdtzqdECRLg8ItdgOutf4ofdecDJwSe2o2ZMHj4Uw8x4Db53rsu5zuD97ReOKNS6eDpde7CyHTzNllxjrdRq%2FlD%2B%2BJwL4IAz2eRjcwGV%2Fl0QV5NgBqVNX76m2Ay46mnUIprXqjDblNJod6tAH5ZQe5DgvqaRj7%2BLE9t47tAd&X-Amz-Signature=433ecdcb6665bb59927868bc0dbbaec79c72ed90f6c6d2c6be4a080ab70a26ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
