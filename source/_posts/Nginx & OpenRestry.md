---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667ZF6346%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHo01tbP0qIplcDAlwVcoolzCFIuugzTK5C%2BBOCRkunFAiBDnbPjaKCap0RfxeD7EGPaG6K%2BjbrWj0v9wDEvrYwvCCqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsrnvyB7m4dZNJhVKtwD0HIT7qFULfICtocFzT3QT6vprfxg96F%2BWBL8HyBy6uVNgpI9nHVkvj7ENkD6LRuIFxqiXOrXw%2FioFkYLtThUlKPQoSIbbNkIzeQ5d95rqhjNp41FQPqnemR60Oi7tZmf1R1bdkBA%2BEjfRIdv9WJXdKFGCo9pI3N843vnGz%2Frauy8xjsBs3tR7GSMy%2BStWRyXSXyngdRdLcUqc%2FrgRZWbyMqUtvqxSZhWkNzEDFCLRwiqo78HQ85UTaCSGDb6p9K0DETXChKiT5%2FG%2FJ9SUTzp%2FoTLqQmmIxd0Zk6z0hIK541xIy%2BGD%2F%2BgYx5ZFbYIQkhfyUDAHHfv7vX%2BXOZdcSFhZIy6X1f8HEyMaskjqWmF9iNz2ystuN28GtXFM5mkvYHVPq%2FXW%2F5k5piqpgUlYrsDy9ctEjS4eRoFWcABrckphz2kdB2m5l2rvM5JNUeBbMfeCpgdoD9Vvr58Aj6NHxVZyyoO6EGE5Tq5J0M7J4or5VSEJBDJS2vOBGBAQ7OohA7AiyMGZQdzLHDoN4bzPBfbvACR6LBJv8duUuxWcfOPNDZKoUJamZb3LLUdz1oydlcorZrBUlxp1g7VnzOJKskmKhwrL5dlqsMi3wKBEYYSI0A8r1XroXmD4PfhIrAwgpfbxwY6pgEh16LwRNCnz4Vkt9XbsPTTEo6eCY7%2Fi5E3E4yLT%2FBu5EHQy6N3vKnZa6ypsSRFm4SpeoDIFZyFm18BAyzFpiDukh1p1cR3a8kbicgdv92e0Uqn%2B8ozO4sVwTwPdYzhXba4D%2BSMObgH7dIcDxrwSovjg77NNLwoPvDRFMj%2FuEbGNbccvPSCk7c3e%2BGsRKyRUAFRLeTkoj%2Bmob73Gh11nY9ahS7KYNKR&X-Amz-Signature=5b94c83d81f4a842d4695742b486b99ac291aab5053f7f412fc4221a0343d985&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
