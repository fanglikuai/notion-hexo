---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q72H3M3G%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJHMEUCIBG8o9h5wyYn0J%2BUUE7Cn9ok1f4AUc7TyF2R4u9nD4jDAiEA9%2Bdry9LzXjOrdxt%2BprgKZGigBqCS6bIh%2FMV%2FcXtz8lYqiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJVN90eqhC5AP4ngxyrcA%2FBwLltON1%2FvAONSMn8vOYzl5UmFU%2BLYNyIbvvpONd3AO7fR9Udcbr4FjZmYbSxr8wpXiTkMRPGSEycalo7x7A5YF%2FnICEVlG0yFuvci%2FXRE7tIbQqow0ZRnjMeY58QOwkBpfOKx0gT48qyg4gfitVkgAwwXYaZLXLGSg9bHFEaR5Tna6wtDPNxVAaNdq3c%2FOc8%2F%2F8UCq2M16Zrqb4%2BXQe69oaWIcI92PwclgO1dyKi%2FoAWhc2gnz1wruk33QFs5tOnSeU2joyZ2T%2BsRiwpRyS%2BBpYw7rOBc%2BW8XDAiPWivBHg4%2BlsSsfi2tjQnCXz%2FJz0aZXBBn%2BU7gRbUX72EbmaNo72dSz31UaDpUb1IE%2BPQcfiedvyQBwnHSMFDhdnwT9Q%2Fxc61rfoR4Ql%2FC1XiM2owq1NTSkQiVI%2FlHPosqg5XCUL3M8pbxHWCjJf2el7LrwE1JeTFNOCErqKbzOTrWGrXw3sgiqFdBqlATFy6jE97hortj1bw2h9JKm%2FYrgMagex8CDVPba209DVMkeR0TrPKF0rY9hyK77knYSeReMHFsHqE%2FxI%2BsV5lO1zSOUueH7TyUzt1KK3rTBxdW1g1fkfFkpy7gful9hhBxw9pD0O37P87P%2BnOLbH5TzzFuMNOeo8cGOqUBHxVJe%2BloIZcVAwn0fUIlOQpYtYN6dctMAwfqK17%2B0XYTpD1N49OSTaoXNYhlBBFPfkpRJCialgG5a2Dgtp6fbNk0Cq1SdsHZSHHhsG4UGLtZOLUU0dwoownbgqE58pl7HZleuiSk2ewCWq8UsuulPudwnlJkq72N%2FJzDJMdpMSACGzIYodD2zgKlqqTquZiBBU2gd1cS71x5ByA0n3TgaWIO6jQY&X-Amz-Signature=972ac2b60e2b8090c4ef99c182b82b6b1dd3c244957803e7e10d914bfed95b2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
