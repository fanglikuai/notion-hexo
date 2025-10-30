---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BP7VFWE%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQC64Ws5HXHqZpDNczYEWSQOwfBrzmte58RrLQqBROT6mgIgWLA7R%2BOjaABK4VKLW8Q%2FJlWWvgnjKytIMpi9lM%2FhP2IqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2FkBejri7ctP0Od6ircA7FkN01U94xWmMwzby8bzffg%2BNzayZruskzfGgaXUiGgX0BRoxwptc5syYAWxDz%2Fn1UvlIVeDhY3oMG%2BT5rPN460Jd54uTqRLi7K9QxolIApWBGQi1MDpewQpExBMLHxupQoWNMVy23ha6ENR69FRcIPZgLBD7r6TJ%2FvSzEZ3rrq2dY%2By8Tgqy7Bx%2F8ohUAV0D8KAXHbwfOwB6Taov30n2377kYSWyy5QA1bKVBzYZGhDH7yHsh8UXOfkhZXhutwnCYttK9CQLBtyvMr7dJsQKWVQpXpLKuTMU%2Fov2MF%2BSOCnSvJEOUrLv4Uj1S49eJzuJXTmjWSDMtn2M8R4597RlOmmAk8D0PhDoNY66%2FLXdSTkbvO%2FqroudFb2wsArSaDk1ThWpiCT8Lmf%2FnMXNPqZqMmatfhNIaBXNr01bSWWij%2F%2BhLabkhtbTzVG41otbvbWAqn2eMdVKgEF20HhzALik1Fg4ugjctfYPv%2FQmq2bfZRIjaSaW1hYemgDGJbsQOGbYlK%2BvYiPxCHxwcHtIzg8HqzJ6vU7WAHSsT1lV5L944OpYo5jcxy%2FXnvMQMUxZ8vsn7GVPJz60StKAtLqG6vR0CZA%2FeWpdJ57JE4RKIMdCDG%2BBsJRTFJv35L%2FVu4MJq1jMgGOqUBk7m3%2BNLCBeul%2BjEHTf3jORwTuDjGA3bWilR90XaaD35PXjqQP0se8DVGLSV3TILBzZWh12cP9UqkYVjs%2BcxIIRgRYieRlNEe2CcthWugMx4azfDWJD0dgCC13UOVqg48Sc9%2FO4Thnwath%2Bif5CacgY3BxyXdmSyh99WVHayI1U%2BE0bWHHLSivrucP5GIwLrgSkzFBUGLrAyRPbhQglkugc%2Fg%2FXwA&X-Amz-Signature=6f0d4c8e6173a910cbf8bfa548470a5519cce9fb547437f5cdfec777d66a68bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
