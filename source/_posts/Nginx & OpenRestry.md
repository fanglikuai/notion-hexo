---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HJA4Y6P%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZ%2FQ5Xcmbcrl6fqLHZRtLvl2hrKS9BrycZZi%2Fy4wa1YQIge4RpUUC1B%2F2d783YHUnA3H%2Buf%2BOa8dwCh075IcU7ivwq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDLz0elLwoY%2B1eiWVQSrcA25okgTVBwN%2BMJP250x5So44PobVGxEc1475hwvFtB6ZPGbCSLLgAuCzu27j1hytsrB%2B%2BynPxQBaJW2QigYpiFb5ZFDbblArdOdy8rMBOHOHtZA1T3RuTU2kYXTfscTpcMiQgUdi0%2Bupg5VDx%2FZULlV3KYcQpBDrGBoKW6nnBIm734tTWEkCtnHmgAWOtqav9BLYfGvGXstz%2B32yyP%2F87IgDXVfGAUQqZQTCQgb47YezW4TT8DzMZU%2Bw9k2MNac06gq2a8xTimtOXaiNX9%2B4hWw3C7HY506ZHsgomjSm2M22NaC2pjb8S0GllHfxrO5OvI%2FQQXWJCXNS%2BF5XuY5fWqj3qYbju84KJIZucHtFtVtvAY%2BR%2BqDbcnMi760VwPIUZPzCbswmDd%2FXJctTAFX4B%2BtfJoWjzrxuyqlWZdMeeevngeumszEATP4LAel9FFAPUWl1LLNKAzvJwDrp0nPVSo4TvsNlFXtQ3Tjn3j1zR9kAABJ0ebn3w3JLrFtnSFOSEZVFU4P0A396U5%2BxaMSYonL6gvSc5k08RnNfVmsazaUGVlhB%2FsPCxnuNdH6AQWuc7pgYif9ahB5M3sGJSQQu9uKrzkos0JE7Ll2IBK5gEFpau0Qzv%2FSq86flWuZrMPret8cGOqUBRJzO1tX2kdfXp2KMVZ%2F6t3IWnaTclM%2BUMNYH5oS3xhTen3MT6mP0fu0IDMmPMSImmoHQkh%2BzYngoqSOQO48yL2SJTC%2F9HyAuRcDPVuimv7u5bZHbbRY0B8lG%2FIakSgBlmnIi1voawyWEoAISpkLL8eQLk%2F8D7Ql5YzLxoG7AL9d%2BGUl13SJqyA7OB5z5yJ%2BW9E05A36sWkZ7q8grZdKgLWjhC5cP&X-Amz-Signature=c2bf192323ba5e3d683a25b00fc1151c135bb23e40b9bdb516c0593771e52d53&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
