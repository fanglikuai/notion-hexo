---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JGUBSOF%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIQCwBQUSHlKKgoyV0ZQ%2BiNa7%2BSmIg5I%2BmAhJ%2Fon4%2B3ywwwIgHO1OAZwhttrX3X2TAKIlw5rdS7QhMLv2QMlK%2BGwagXkqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIzTuZz1w5Rb8Zw4KCrcA8GM4VOy69psHKNn%2FwDxJxi7CcUxmPu2rVJHtHmEoi6ORRp%2F33ngBgHJb%2F%2BTp391fCx3YtdBufvUyqZoerED2pNCthnBLH8pnOwTe3seQg%2F0F4gJpHI%2FdjD7uY%2BrQqtWhnBNjXb5FnUMRQ7oaTVx85kUrdHVSUQzmRiWGX6uMHDw7gH01lZYoQ18%2BpHlEwl88APFsXPbjq9vg27P5lUeMeAnOPBb4qeZyi18tS4R5Qg0TWN6tuDI4wmNOUYPT2t%2F%2Fq8Pgj4PONxcWqygqzeJG5XEbxSiZfoIva%2BUiyTHsOmScVe2pSCECFr7H9YljF8uzc2mSd1paY6EvySYEaE1%2FWUFe4IkPdcgCyXAK%2FL4BFAjwZTJh3BD1u5WBaX9bKnx1n2J60AbNfU604W%2FONNyfpRIapOW%2BbzxKuVRJtAMIkbncfFODyZVdhsqf%2BDsesoohjlZTlTjVghlQu9I3osQaj4h5nePNQurGITSqv%2FGbhZBcmF1EYL4SNb%2FoI0l20L4K96TElDJPS8eSEhYSY0YoTASx4o9v%2FdowZFdsY4uHOYy7rLbPmWw13pR1cFOktLK2d5i8XdcURwJqUr7mZOupUecAEGAvoFQT3gu5JTlPdVbs75V06gMeCDpeu0NMMDwlscGOqUBglx%2FBTRh4V7N%2F202U6ge0trV29igy%2Bvd4x8x8OJ0t%2F3vaJQRXrBPArSXzqk%2B5N4lIX0Ll9O1AbYITV999jrEoqaU4qgyuBvQGwKV%2BrH1wN7w5Kv8PQCHNQ2Z2PmMtg1QDQq8UQkbHFWUAlBKrHcSrtGhlvEKrkCe7KNpWo3t37PPblZyWEo9NCT%2F1aGuJ9joW8F2uvgv0JEkVJgCZG6xp%2BKsy3uB&X-Amz-Signature=59d0ba8e6ee4a77eb1600c4c7f08c77ed14002defa78f290ae2bc2ce3f3cea02&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
