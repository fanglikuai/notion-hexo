---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUE5342A%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIHEOlYTlqnM9ryNzdGfLjW9IeZDzRtBGZaolAX8p68mjAiEA3La6qxz7qkrdUoGVeVSWArjm16MqJkRXtOTNdpWCEEUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsU0R6P0Qriv1oCZSrcA9MK20qPijQ04u1wlHU4dB6keJk63CLxzJ14trb%2BL07VsF%2BYBbHoDPHN3QN7Gu6SivAr9hW62g8%2BlVMU9bRByM%2FmLTMqkCLQx8Wb9d606cP5dj9NdmgY3vYznE%2BuW8227gAMPjPug41nFxUEJ%2B%2Blk4G65adzOElmbvmq17dwEKZKqO%2BFYDo99Vzp6f5yt%2F2bnbDQm9nlZymT472W5RKu6IrE%2FW8rtRx%2FcRTWEF2ApRgOFsSnrSay7R6%2B8n%2FhgablpTYmm6s4DU8AFsFKC3hWIHoDm2I%2Flj8dHSRRXGGveONRQ%2BbT2GcS%2FOIqGny1mDhZW7KmS%2FxitiFPn%2FqUxhx6AHzXBkCJ55r5KnF2SJuCNM9B0WspOBCGYzu3itru6ScXgSuv3N41XO%2FxVrXrgzh7%2BEx0Sa5%2BvsdRU8fuDvxaT%2Bjar3lIa9njpxJT1%2FZ2KyTVt0gBdB%2B6MIIU4UPAqhi0updbvpZZu9jXQxESt%2FkUyxPrUQvZPnft%2BM%2B0LeixCeCkdwAH853K%2B%2F84K0XhloDRME5JnijIbeqefhqKRzKG89ElPVQo7c5undePpNkUrBnhSbc0VUo6ujPA281C%2BTos%2FJlp2Yl276fOPxlDNjA4M%2FQA8Uav36%2FoXhTiWekVMMCf2cYGOqUBhOpVDmMIAwThsuLjc5z4kG%2F1q4tBXVOT%2FGTD%2FdtZHKxSjOVjAVczkShY9X7JioayCdF80YH5GV6PGRaMy3oJZFLhDXkbTCmwPbzDrLxkJCVclE8PN2A6zmnmIROOkkgRArdUIyrvUbPhMzv3qD5NgunHofLOBv45U99eFSctGNflomfopy0kPBH4XuErH7vvFrvO7ro9vO6gZVJ%2Ff93T2SlgxBkl&X-Amz-Signature=8c9b3d40257d9b23a7089174d4effc77b6d3c2c4a1e2cb0893db4f749473fae4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
