---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663A6EZZ5X%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIHSnYFv8oWDSAZubxKEW%2BC77B4XQ7%2BwhDqP04Kk8hu%2FjAiAjLXzMYhQc04KAAZIiJWejeUTn5IstqVG6xcR2llzaYyqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAtWa1GmIZN%2BkVxW9KtwDF5N9Gk6NAn0VfeV7sYoUYT1QAwS1DUOLHkokSzCFQuYh3bAq9e7%2B%2F%2F54X0BGouqeEMgWaROFZcLTHed0zTaWC99Yg27khXCO42Tm0PvLmpKx32NleHw67twOZwYs8sBVRYmfNONke6SSHy7gMh2PkEcNW4MQ5%2B%2FKMN4i06TDo7u5yZTRdq1lqTkaG7GJ4s83lnnfpLelSviPnlQHJaBaQPAeua5g0rfjEdUNswWo20ED6UB%2FaGrn5xen9k3zxQEdZkQXyn3liK12r1wSrgR4AMAS3FKavVH8WIvyt%2B7n0v7n%2F4hvi%2FXCZX7u4I0471J4686c9PPTofM3DCZjtIPNBuHlJVsMG4F2zaKneZ8BtG14G1pHL%2FPJh7LrAakrlWOIhv9YM4vvZ3L%2F2tOvSHVu1eGOjcIpQ%2BRYeX75S%2BY9yxVuNFIdS6vUXbnwXJfRc%2B0bItqXVTFWT2a5KXETVIZoAGDVqdI9A1kfgwgns0wj5tvUWIuPmCCm6pVUt%2FM2QkhEWGY8SOLg36s%2BNGpOeh1TVy8uUKKCxtNDPnQV5ULQZuNi2HClDh6WUjRQw%2FTmRtektnAYPMu%2BKAfv4w3xRG7DEECaWE6qnic3QvaU9jhxa7Nh%2Fmpo9d1jD2rtpAAwu6vnxgY6pgFQVfj65y0wie09xCkfzL9whyqaaftpORp8xoEbfBUXfZVZKoeHl%2BkiYir9V2MGqEmJ%2B5uyb9%2Fz99fyUNOUYrGOlXVDu1C1J7AAcrgnNWo7N%2FNsHwdMob3yjbvZ4qZ3PIqlvUwxH9X9RF6mlQuH4CGhtmwVcatTGL1L%2Bs3YoYDMuPba2nzulMu5i%2FRI%2FKtE6WftHEelOAmsO4bdXzSpjAzyOr0qm%2Bvi&X-Amz-Signature=8d1511e73f74e14e72ec9a358ce43fd24beea72937d4e7b469bc3a737ad000bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
