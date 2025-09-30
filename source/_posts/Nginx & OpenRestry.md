---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLLSKNI7%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQD2fL%2Fk6WlWlWvqc%2B6b4p377RaafZVkoTJaYOtL0T1eNgIgGBSyPQoJa%2BriCGcz7xRzs1Es0zTPqsiBSgPc5ddzX04qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMqw%2BoQ%2FutcLPOkUyrcA8L3EujGp9QfnNbCmKe7ME4JUh0u4eDg8Z9hAP7wQ%2FOiK99wqj2R8SYsqDCuK%2FGlSwB126Y47HOrdYA7RgvMswPxiWNWjKPiqeXBC9HH6bnv9YwkFrLaFi%2BbQDGTfA6G%2BORa9CUvq5ch%2FbYDyncoY%2FmrkgRHfkLtUs7ntHiTeHw3eG1vopiVAOruzwswpan4o253aLEjWKcI53RUX0hKU%2FLiBQCCmaOGuWTX9LONAT0S16jJQvksTGq0Lm2KzJuwrLDq0gKUojPYklMjUvzt6s3Cr4kzauSgUzhDs5fJv6hUew1FAe20R1dJCzvqLjZgjc0k08Wo0uE1RYLrV94ZdI8Sw82%2Fn1rhM4BEkS1aD7hmm4xpVxYMIBaVb6zpEwhQJxfUpySve%2F9wgJ7%2F6cVcOMv7PLl4H3A1U%2BEGBhJZuFmVe%2BVHl18d%2FFKiSXAHsBWKKw8DM2u60KjIgp4K70RGbkWHqrI9%2FoMnS7xcJEuPZu9o3av%2F3ftgLkwn0XyJDG%2Bk%2BlYzJajS4QdnM6i6oTg0WvA3OKh%2BQTQzhB6lLmw77ZOL5vG5HvQPkpd6%2FA8VDDPYUbt7fLZUGTiUJJfEZSSa9vP3yEQik29abCNxeFdFaNNVcqmLMTsIIOahjcWVMJeX8MYGOqUBBLNu1peD%2FdL5xdHKLUz8rMC3vJ5K3n%2Fp%2Byps7j0Lv16y9X7aUGuKxYix81yzWNHcPhqZ37PksWlIGOTIOhZDEcrUqJ9CLbj0pohSycmM0pX6aFiGbjvol6KB6h%2FL6OQL45oQApLhBDpOktqkbGIEXVi6Zu6EIbEwjXOepxjZ2468aH4FgpOBAFIhvLFLKvFxYGLq4NrB5z1wnVWd%2BvaBMAFKZ41I&X-Amz-Signature=fbdb855c428299d73d177aa7a933643f11c407fbc300e4c6180b6ae6c5329c2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
