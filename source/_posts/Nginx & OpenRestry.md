---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THTAUXPA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T070053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGugFLps13TF8uUh6i%2FeTThRmzoWQ2SH1KRQ3llaVNUAAiB4qDVZIOTlVi53POHalll8aAEk95vtT%2B9d70kighs6liqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaZbznCs%2F8D%2F8E%2FS5KtwDPO8A6%2F0mUx9dv4VYt7giWmpojGWqucmtWY7MnpPnRvi%2BTh4smQCZnTG5wW0OZnuj5nJRq7f9Syf5csjYFIfxdVVP3Ofp75tL6w1ti%2BP6q98lHxzbr%2F57qb2ZLmRFyodIPa0RvN5U1WKTAcHKiulPSnGSRf85nZpgnRoIBKnDTmjmbEP5ojCbPA8Bs3iWHLCISBrCkMI%2FDwt%2BhK9dTbKCzoRyxcZSdTs3VEa6hEXIYhNmUNgg1wL7eXWIofbFRD0%2FJbDyMfy2e4xX0T1jPurisY%2BjOjDEW4AfQ2wb2u%2BjdKdG5qPiSOs%2FAxdZUwR0x6XgdyrU6qlu6GaOGgrNEgi6ie8tn7E%2F4QxgsqJ8VV3KCjOzzEPdCTnRo7wdLp8OcewFesi6DABQ0UWwdQPpDslLQX369qoLML23mw90neLoAJIoM0igRsoDQ3zdnyHbIBw1KE8z9wqbFNew9QdFgkJXv7pPHG107n3HdVRHwmrck9%2BPu7lK07bz6Spjfo0f%2B2VW5pAXPMAZN%2B08y%2FskCRc2q43FYNdhAwTNuCVjUlxeJag5%2FL6CLRbbq0%2BdjuEUc6jpoPvMny0x6bMgWxWTmOE5rIl%2BmKkTUUXHmiNzYboTurlkmViu4bJj10jeVP0w%2FNL1yAY6pgEsSQLn8NFaudWgI3HLBtO%2B%2Ft5lizckd%2FQElGW3a6lIBxli57xqMpg5v2k4YWdhUQROHiZkV4TKKldBN%2FXz2g%2FEM6jH4pL6tPw6svGEvPPuJcQGhLdyV2Gz9YmXDV%2BYp58CI5BdVKoawAGwJjd6Z3F1QCEpbCximG9XANzugiF7kSLWhUDnHe2JgI%2BQ2AJ6dBgv1uQqULOadWlIQ5uZBx6YC%2Bt6aORo&X-Amz-Signature=2f76b35ab9f03932ea7709ff84273f98038fe0ae701b0068ad4fc56d7cd3a084&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
