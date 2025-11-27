---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMRRZYO5%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBm72T31ZAFDXOkOni%2FcAe6jubnXWnhdEbRkE1XtrFslAiEA1kYXdJ8Mvdw64V135W3g3aJC2LhdEK2y1aDTbMBqhG0qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDF%2FMaaYSTRsPGVOgircA6cj%2FLD0O2GifmxORFahwFEZBjTbnloT5YwVJY%2FjS%2FZ7O%2BtaGWXx702U9sYKDvGm%2F%2BMKm9ucMIRI4zY8%2FFgGeBq4N6P2PjP1u1BPuNhMk%2Fztz2F8fdWfgAyCxFZEK3pZanQ%2Fr79HuPxktYMoZ1fci2N%2BHccOrpEGGuFP6QQAAvA7MsHW%2BaeCBIfwWXnN0zGqiMWY1hzD3%2BwmhLwQNZ2p9i8dCPKE9Wf6JJLjcFBfQnxI6EN7%2FQjFK%2FWSNzOXYSqNHUMTqKzZ7eAZNBdQmH7d%2FRP2%2BWXywrlLhU4jRTeo%2FVuVdZB430GV7nS8Rwg7vr6tcmVHEUhIrpA53aW6J%2BBMo1CmMh9I8T%2BCN5AygTQiiJwwpDf5YgdrOafoyTI0LrRzEZG7dlgPr7mjaB2IWa77%2FFlgcKJSCciR2wUuuZyBM5WrL0pTPmoxOlBOCK%2F%2FqPPwwb%2BNeSzyWM1GgIOVepGi0gCL1RyByQP2e8nH3kGl7zTbxm28i0q7lUQoe5ZaCumigq5Jtly3ixAeoB2mNiKK24d2who2PGSU3dl0lJ%2BaWQtKHBO%2FPuEoYf9j%2B05FDMucNabm7TD5ww8QooG2O3wUrsJEv9SQQPZxsGXZ5S5r%2FHcOu3nmjA4IaSsezsc0MJu4nskGOqUBjniCZiku%2FJRJdzU6Jrxx5XGJCAPfJMhB61nT0PWBJN2qnqZ2pAG2Mszsh9eRgD1hxLBnAywDMfdPDkBwlho8aTomWsmWyZQZ%2BMLv8%2FSEcoPxBc1wzzUhGp3evN32xxtb7Y1kbwS8PEOG7DzWC8NS9%2BKf%2FxJbmq96C%2B0yZnosBsIwYZwwGtUKYA8XQVX64YqAt6jvQ5CdHKFaasBWoRdYd9v2sYy9&X-Amz-Signature=86b9e3219ca777cfed02297c554e0cd554552edfe7efb80311049614bd424b1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
