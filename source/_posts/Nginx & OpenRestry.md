---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LJLY6HC%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQCnaC7M%2FzVMVZJCIt0ID78HStx0kDZYb4HhSLlcoxtaVAIgeSm5ooSegQ5isRhAC8w3kRc2spjLgmpK9o06gJrQy4wqiAQI8P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIaJygwyLxEfXhrP%2BircA7LyYrn86FyM%2FbT5l1Sn%2Fbu5YVX%2BVEGvhioixYQbJd%2BMNSg7T1p7nWA0NjN09N8%2FyKgUySbfk14qvGKWON8tu2s%2FWYWXCx%2B0ZQi7GMJGH8mbNK%2BdPZcbW3sU4DooHbXjRvUmyt7Xgvvmi60LNgZumHSU2zwxst9DFSdnqXXcUgjEjfH0ubQyLL3AMrxormSKf3Ae3L0J5HyT6yVV4TT%2FtOkq%2F5OFCusUVs%2FcyrfdYrDsgGxTr6Of6HZUK5yRIgQMrKoaCBVbIv7lR%2BR1mcAnHhfA9KU98CXJRTmvRV1d8OFRh6JrJaA5ar06VrJzN0bGJR2J6u%2FPG5qeSAsB8TyL%2BEf5PXaC5RBALiIyOxEvLHj%2F%2Fsprf4N6vJIpukUOIwQiMWxidT%2BLnJM%2F3O78Mol5ZdAN0jrJYo2VBg0%2FwLQjk2XNAkANPEggdKNjmbRcpY4qDywgiiaXjUPpqt45G0%2FLA%2BkyoFul3C1PJi3vMubCXzVHofHiwiVfsCPhYyk9MOuxIO36K9l%2FXpGU39wpaffWSbyKJxjSXoEgWysNNxu%2BA5GtsMRoK0HFscty%2BTghjxaG03g%2FugiTmZKVupAcUXTpfZVNF6yZcM3oykyt9qMzWvhyfv94HZ3N9rSjxoMNMIOS2ccGOqUBwEqws0MKhLFk2HxhREDqb1wsVAMQg24mDznJ8OWvIegYVI%2BtCwKw4O7wZuwKrsKoDebqfVov7xuU7j%2FgCL63q7r7VbofslVPPu6pbbqi7t%2FaRqhHSnDfnQ22xisWWHo7m%2Bv0S13%2BEqLP6uveqAbzJ0D9iL5xFPxClivbYbK48e32FSFADt2izY6dXXKpY%2B8CQtZ0iMa7XGw7h9u%2BlZ%2BSA3B%2ForUb&X-Amz-Signature=73e917e31150c3e283a6854cd16318073ad7547d42d2782a69b363fbf93d30e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
