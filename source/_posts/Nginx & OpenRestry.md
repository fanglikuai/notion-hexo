---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W4LO2AQU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC11qxrXiH656JwrCnMfxsP2%2F6zldpsOT7gDerruejScwIgNNWrWYFhG8pLEpFNiDZfTZ%2B%2BAqKqm8GsFMngCXDZWgAq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDMMAxqhLX4qqvzxo9CrcA5vzmRvPYkh9rsS5MGNKOrKLbW6c%2BKD33tjDETzNymbR6xKMKj1w%2FVnmnxokL1jxaKsb4zEEsP98XvufVTZjbPDfR%2B%2BxQ%2FlPqbh5xfbXxhDcxiE9sWSnVOdLINNolWRqrjeGC191uiczyHJKvCwLa4TSPg9yFAa3sOTd7VXP1s%2FmAURLd9cwBZFwAtLD5nAE0H7ALpKTMMAkS1f9iE7agNL42QrxDrvlGTBr9UrfwpTKbeSk15MUc82aHsZ6x8F%2Fq1Zr2%2FBdMIpdRwhBguqDDcAu91fTWHOT9HykGnoQ43TaKkgt6xDBVJjVKS7%2BtetIMdyPnhCb%2F0gohRnJyryG95JPXZam%2BeeosWLqSG8MfcaFd27jvFpiDvWI0ACjJwkOnYuiJDKwaWCi5BotfxDLX0R2b5IbUDunMGuQuB4Rsx62Y%2BCi0qhOyU9yU2Li2RCLKEKqOPvhE%2FfDWR0o4Sf90pCiXcVwDgHCmmUEafN0QOEbyJ%2BTQm560sO%2FuLrOiiCoL6%2Bq9WkLPQMxztbo4J%2FQx0yvRxy2N2%2F%2BLrKblk%2Fxh1U8I5U2DN23vaXVYw1%2FpRcacHGHhG03gY3p27fFlJsSmmoUIBpcx5fxJ%2F8J4VXN3dc63XL3J9GSlbHwnPy6MI%2FapMgGOqUBRdPTUS0MRwVRQgAJgAKjZdAcFFLsHp8ud3Qu2tJSLQcVyo28SukigjeK7bL%2BGalde2ASbH2XVxgZ4K%2FLCt7jZprYQQa7DNeF21cb9GF5OM0nGU71pkM8Ili9o%2F%2Fqz7RPmWC5Xva07XAZu5%2Bze1o0xWEoOUY2NeH8JTYaQhWWSgPVl07XMKBPj%2FYZcVZHv9xI70pOFhY4zogdHe%2BYDdg3lLO5nCtC&X-Amz-Signature=f470732c088715ee29c18a34f82b6e215573cadcc70eb020a9161e2a1fb83b9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
