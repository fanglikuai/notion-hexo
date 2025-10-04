---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YB42XXCR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9WNCD0EY1qhoQG2JQgSnb94M8vfzaJ3UPrbaEqN45EQIgWWuM2zmIsNeYW0o2%2FyGGxO70F4fGDlPZWaKAtUWAOz0q%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDPob%2F%2FhSSVOCZYR60CrcAxouhVehJ9qYueh52OrAplZ%2F8w8Bztv%2BGgs90jq%2BFY0ny%2FZmuLY4RKlfVt6Z4O3wTvIfuSWD2vREN%2FMPDEuKr4XqAqYyHzIY6CEDBW2B3xt0QBj%2Fv72Ol4zgdVwYvLG4f1U8pJZIXsMBh68AhkHAPntU8VdSyqrYW4ofYjdYaV7rG%2F85zr6N%2F1DXBahaea43Sg050zuwWd1oF9UsRvQTE%2BG%2BmoisFWRSFku2I1i7SswmH1DR%2BeXPXauWRVKk4Q4jh61P9BOAPvB0BIVihL61sHATrrfjS14chqhy7W2yQXS2XKSkfxMEx3oskGTG9Du6Mq6WMm7ZMlv32DJDXw9%2FbRqGXOu5h9Haqd5316oSOVMq66Vb0h964TXNuhulZgRu4LZAUroPE%2B%2FE0EQ1emxBXTeb3Aolqrw%2FyMP1c1Y2ilm49TaKNDgTeltYctCdzT3tePzluXZKh0njcY9yuIrX7%2Bf%2FnbXWTuFJDUoKgKFKDTTswcXh2xhKiwxIfDpsfz9mQKlT2AEOBlaAmSnrWF8HamWyCi75AD0hh2tNxh8779032P55lM4ie1JJbo8HuIS9s3o0mu2ZaYoeFSG4gpMQgazON1A7yuPcjB%2Fyri30S2zv%2FnF6KAucmqthGLwnMN2vgccGOqUBe3OF0llrr%2BgR4dUsCPM1xaNrrhm6of9zUKPvqS3p4JMaOfKPiMnjis3%2F432syaTXZsRxAQhjvJtuXVFpXKjvg%2FrRHCTq5JlgEKKZ7lqTnmjE3pCC%2BSA1d92UVegxd9bR%2Fl4GJfZWjdJNdMMlesvO8CuWnv2TiWTK%2BGMAkcBc0x2jw9%2Brp12WpntgnHS%2FQPZmZJ6qvEstRZMWb9%2FhFmBsJzQPpDBb&X-Amz-Signature=71de11bebff6b979d3ef47d3de8b0b3dee81bf3e1cb0418dc8f02ca35332d845&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
