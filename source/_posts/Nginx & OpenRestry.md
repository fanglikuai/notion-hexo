---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SIM76YP%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQCLrOlUH%2BPr3Kx9YhcVA2FzNnSEZHtoDDZY%2BM1OnbohMwIgV3CnSA3Van9lGipnHLHIaeg8dDt5Ddbbe6MUYMgaTTsqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBHpzl2wKesvyycHVSrcA7fmb1XNcWIKANbluk%2B%2F7dZKfxsmqnn85XdAGv8IOsjurm4TFSf7qYoUqJPSnHYOEnKtgE%2B24GFzfGibYRW686JQ5vYslzf9ZBzH%2Fjke8%2Fqdx1EwHMCMYZRA%2F5XscgxsfndFHSjo%2FS1VQlGplgie2mMhkwj0aXZP0Yl9FBeV%2FrpcGZA0oRBfBlmWNNhr4AWaKDtdjCZwMUl1IYn03jZYJbWXQpxq0jqY%2BVfwMbwoIlbGXYrOkMr8r4sDZuHQ8eF9M0O0pb8p2LuTXzpGJ3NilooRP5GvJ%2F8nYWluv4r8B9MUzdsqgiHdugDGeh2JGhIWu3UbcuME6xqjoxzAy8bAc7n%2Fi7pkks7Ugn4eRQxe1lznb%2FwLUaGUnNXrC57%2BIhZSHWT3rHJ8HN8%2FuGrIvk2LFWDCs8cT9%2BkTk%2Ba%2BBykdB9%2B10G4tZ6zHj02t%2FSi5XKcRHz1Vd04z0mC1%2FTI9CIHkapHS%2B954IvMeLV%2BUJ7wUDGmCYzs5q7hXVUWZQAFrtYSs1GhD3TiiG964Ef9Igx02xttAKtAEZ6cBoGDMrobWkMWjUP7CXYreuFmuufw0cJnLkbXdort9GVnKTncpoCPrdwjv3428qHIuPyDswxe4qXu0j5YEgZRCOUAHQ0DQMM6X0scGOqUBhafGMwLVF1xvKwMsITa4E9qmCEpiG%2By8vZkoE8coBzSot8i%2F7ktkAiBUuLgTkXRxuTQqpx2455iEWOhFHJJB8p1tR5%2FocGcid669Pdj6TPV7wezV%2Fvfa%2FRQeD1zh8UymhLUGZPHaESA65EEvfeWRKQume%2F7mF6l9OtV2rTDVYXXieNSTnLnityxJB45CQIy1ZgYvhxoUsxNYqbHOk0KYPWFj2h%2Fb&X-Amz-Signature=e4a4846dd6e9029ab2c284cd8d4309a3b88c7e1ef0572216f459e7d0eef171ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
