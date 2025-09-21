---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DWQSDDC%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqaNWzcQtoANzNzd3vizcJ%2B4URyXkybkYKQ1EFiJg1cwIgMMsIgk0i5M1%2BTu1fFI4NHmkr2c4MQaM4qa5YDCdEUT4q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDHhuLC%2FcjUko1oY8mSrcA2p88b7JO87dJQjdKs3i0dbXGuX8HMsxmmzXM47bJUKu0OJgn2QnkO3qXtyYuhefs32ZCOaoT4s8avi95PJEhhhbvncfND2SalLeimjFUuJPbDfAQCrMdI8KWVzUv7T2Eh444UC%2BRHvo%2BKg8rasLlX9n%2BEe36j%2BqPPlk%2FEiHCiqRBSVLMz5njy2MqvVjEGLvQrtwseSXyvdrNzGcJQP3xNv10fsXN%2FrD1nKZ1mjxspEVAwnmDu1%2BE74eiFvopH6mC0qeOcWy6lol8QZIxw6nO4LHsxe6oRRtfffIEpManhFIyNgQ6E%2BBOhC3Tn45vAE5QmymBRhiO%2FACeaJOcCy0ujwvukjK4Zq9ZV7blsO5M9uzKkzcXO5pKYKcJ38XEmuXZNZ6LRWuM%2BLvJ7teYidYeuI%2FP4OJOymJ4bRtx0g%2BBYUn4Gn2qTpuLMYp0TpSPXPl5g%2BywFMd%2FfhhCfijd1F9Gbi0N31od3%2FumAVrcQUOvJy4OFR2E%2FwVumP%2BY320l21cu3kQSBuYwNOlP0g0GmZHuqVr78A5P78GUUScaWcSk4j%2F9mt1uZqNoc%2BngN59mo%2FInX8feNPtAZVVOOQLbD2Ueh2MkG90ALIpP4f0dIg2F6gllMxkNI4MrMj22cVWMK2kv8YGOqUByA5J14bA6jHqjRCAUQa7oscgYHiMP%2FQmRa%2FzZe4PP7NAxumOJqSMaEM9GScFL0Fj2ffKWFyqMmU%2BWstNQVaC0YXfxIx5htfGN6zkiXvNrfNIXBcSsnx7ilpCy8wakSdONx8%2FwfQFup8jrXz3zcmNQ6E7Cx1uL7wOOl2jvFNjf%2FYbGS01hgaowJnbCIUPgJQ36Wda3%2BqRzAXKYpjjWtj%2BURKa1hup&X-Amz-Signature=ec1e7ae4172e5ae58772a664f85353f190956e0b7adffb128a7b010c26d50f0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
