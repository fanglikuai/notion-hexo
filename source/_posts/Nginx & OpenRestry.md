---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665B2KH5BW%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T110052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIDqFulw5y35m%2BKoJsQO7CHNKhtloZy41LRMeBs1jqxGRAiEAjpuPEssHCBs3TsJKDJ3odPrkdZXaBr9RzhPsYN4L9osqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOndRjt%2BcSOT%2F3VMsCrcA1Mro5cs1OPb06%2BUkRZk1jyeBzcs0dcr83S5cr5CeHhzMUxqrsLWv9Ro5VFzfrFRnIzYWa5lKgeJ7uwRSqoBbAD7nznlOUhOSWk06kmKInmZtOW0hQs5fjaIhdltP%2FOXFPRVRK72c8wgXf82xtuIj5T2VqFBtX8Om2vNG7vKC11j6Lmsro2ZTs9NV2AvNpDyDrHeNThstS1M%2FnWvBS0FIc%2FVkqinFKpljtwnoUkmc5G%2Bk7qvZLr3r5WVax99QkU0NR2eVz4FNetW63f5gqU09HReHa7SCCiEN35GNUJ1aRiOIV5BZGmcBLdQcvUutwJBD3qfmQ4njaO7YdLNzY%2F4KriL3j4KfqYGV8X7%2BPCCWZUSOScO27ONP1xLUvR3Cr0Ee3GdG7xVNQBZhVONJioIR1chXHqoJWdY3D95b6NGIHEekEjvJxyXb4zuU1wtXP3vXPumToQVJBBIrsnPcNVkh1LMk80UFw0jcpG5kZbx5PgUBfNPiAxNOGMlwMEKu9uQRcVl9T%2F%2F%2BHnRKV1eL4%2FRWVeZyPX%2BvEuNCvA4weF9QwWzojnBO7aDnQzq0hIbhF3OG29LPYF%2FrQFiyiXO8YpXISSepcmE2szzylrPn3LiRnaFY7DGSrDZSkDclBaGMNKRqsYGOqUBKqbM6qlx1jMxZytB%2F6Z0GcmkdfKzH8S9j%2BbegW3JqtRuhM34Sh3prWfFA5YB7j3qLSvncoMz6tojTEs8TAOsBN%2BJ%2B4XMOwKGwF1v%2F%2FF%2FjyDWYFrg7PojRnIa1TSo8AhjB%2Bq39IcyS%2FkTKjIoUG9lCRoYiZb6RpOOiTwBx5uziVtqY5tRoiLf2jmNS1Jr83OvFf%2B%2BiDpklSzE2XrGRGR4AwHN24hc&X-Amz-Signature=21c17aebbf25b241b4d4f9ae7edb4eed8e5fe0cc4462561b9c2246cfa96147cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
