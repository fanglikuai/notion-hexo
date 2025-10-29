---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZF7GQOHL%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCujlERI2c2MR2oU%2BtByKrtQm%2Bgo0DCjqqnKUKFIJ0ekQIhAKcGMWjbbFXBXDq7VVw%2BW5JME2RhP0Z1uYfPckAdJnayKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtS7kKMmQT87m%2BwwAq3AP1qp2GKgdjVvUA3EZjsIfEaom4s%2BRWcWEENvlqCQQeWcdVu%2B4KK3FEIqwO8aXDfa%2F8AWI%2FNn7gKnnhXIGAF%2FR4iTQDrxo9VSeb%2Btm9hN9nY1Y6C3xFpQ2gmJYJBznLWSPISSQMj9f0uUeDTmisvZLKXxOIOCtwX7eqWLUdBZDQTRxVIL6e9nPfFwLCzVZppTjR3zBKeMyBYtHr%2BIbhAhJdYJfQ2b1Ty74Z7XUBzQOjyEPfkcIYsoDXhTeTLtV25cj4DAdK5qUzrymL7GDEyrAyO%2FoSDtGl%2BxGhFrOQiAk0ffhM9cJrA4SrQXmLwGqpP%2FTbDculooIS8%2BemQEbhevfjzs0%2FsOcTFlUQQrVcTs4xL4EID5WJ9Agetlp2fSM1tWmY0NRMTLH%2FK0sWuX8jo7YSKMYEzelQH%2BVK2K8%2B5DVJMNLWhDB%2BH8dTEnFJrM8WV5ocQeKyhTVUT%2BKYaLDq%2B0uD%2FhSHHs0g3j4mPNCN8GK1zW5LP9KcKQKv6sOz9u3xb2FWALr7fmc%2Bb1NtNcZjSYOlQq3Z3DSmEfaf8icyH5nP%2BYND1kityOiLGkjD55OZQV7UdmVSoTHPWgqhgtJ6dFhcZEJmhNFSBZy0KHfmc6gBZm9093bSAUVM3CbiLDC0yYfIBjqkAc7UKs%2FckL%2BmP%2FdUblYceMktpIFA5ZnTP%2FSOzc1N26miWpOFWQ%2ByO0jFEd2gpYiCMuLksFgZt4P7Q3lqf%2B6ME3xGP49r813PLyuWRVGMQh551GJJnvKMA7fDrZ03zMRDug2sF4GFVUP%2Bby7YHIYzKnS4ymPJXEwYmvNURBb%2B%2F%2BmDe7LuLLE18xU4twUYFQdImbds4xS66n6cpNlA%2BE4BGY0wjGE0&X-Amz-Signature=aa968b1757613b57308cd64c8c6edc24e42c28b6754a03d2ba53bfa42c0436ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
