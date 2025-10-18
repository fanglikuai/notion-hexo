---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2POHWOQ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIAN6aLLcUDHJen%2BzNR%2FyH0YLg4E67u3%2Bo2PCVYGlI6g0AiEAyF7n9gVd3nBqLGnK0BY8aqEBW6oSwfSBjKbdLaAehRAqiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPolKqls0p0zQcoA1CrcA9EjWlDmEZIWgZwgjodwW6y4LPypZeioZPX0OGoeUk3pDEWS6gvtuUDIQhPwns%2B9qjJcYA9W9XVDJTvU1WIY0NnZI0fGrYm0xl07TM4cFM35l%2Bv4RmcLoqOifTw%2BKMGgUYX4ABqtdG8sxJEtTe3DPhaqKXW48hsNbQ2PKOz6w%2Bia8dY4kkZei7JMdsiBjofgyLTxu0YnYmP33uRDZD8dy5oPd4I2pwE31Neob%2BmU6mljW%2BiXrXk6ZfgarQF%2FiFmdCWwi2UHsE%2BtUqI%2FeoGxNvd%2FBgDaYGFm%2FAbhCiO5fo7SPs%2FvQnx%2FAGLLkidIohRw0fcvIxYr2RqIdMif4FFITUnCBWXq0aHgmFKwMKc7%2FHuPhkWBUggd6%2BDQLUiYJbP1%2BRV4deQL0KXsDYtUZ0hHPQTkV6IAH%2Fm%2FOmh4nSeCoxZwo2RynyNKToU3%2BUZOEQSFgyEYmA8W1SObIfteuKPkH7x%2ByrV0FAAIrB3mUOm%2F5n6%2ByvVJvzx9DIr0beoig6FZpv6YfjDpUGebzoe8LU%2FN2XmSYwHe9nb0C1xtjKuhZFdS7%2BAq%2FitUJBzF%2F%2F1m2c%2F3Yy5cWxym23TGUA33WUkucSXgYKSO0bjYsbWG2CEpBwMenfWPG4SDibALHooKlMOynz8cGOqUB6qf4MvpYx%2B8eBjG5rCRkOHqVd0DgyG1Y6sOgiNoYRBZSPNimvrflxJparlSYrzEaaMM9bf43p%2B92yUTVKhTUxFWJiXoKA%2BOEB1%2FrHAcReqmzVLjwvoF%2F6pRFjpRJRQaNUjrS%2BqsMNmmd%2BHvfag9RK9d1EEjfZ20SoOk8YQw1fXhrB1tcroS3eYtW6nWQB%2BC5O5VqmO4829MjFgmEbIfQsNRyMcYM&X-Amz-Signature=3c3610c87ed36e8eaaf63a43c3d0e2328048a2f616082966efe8e1f5c7b57085&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
