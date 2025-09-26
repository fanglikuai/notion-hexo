---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6YKDKKB%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIDBKOYPQ2gdPRurI7jBARQ5oR%2F2yFc7b7mezmMFNQ5W3AiEAwtUb0n5AWhoQlhL74Dobee6hplaOhyXXNn0ucbenpvgqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBThj0sZzanCid2FRSrcA58gcu8%2BIg7hcPEQf8o4Dc5kDqumhx01jC2Fg5DGznZLieZOPgclcecqOiJjjhl8H05Y2dwUBlvMzIbPmOj%2Fx12Ns950i%2Fz4zqLW5jYTK90qZSq0hzEBTQ3EDJbUEwBbFRZ2VK9GzPN4MVEXsYtECQqjYgWuBrlDT7gMrg9l96c%2FZT8GDtiZjKinYJxeWJOgXuW7vu4HdH5WrDaX3dFrN9Lb%2BMtTWv%2BMlvY35qePkM0rAmPQv9wiyvZSUTIeZqD%2Bz2TL7Y7ZMZ7zByPpcpMQBxFi75vYKcbMPOA00bmDHMYS0EKzJb1B1DNg%2FQWTJXn9qQlHMnZV1mof9i182qz%2FdbWBMNJSAIhwav2wDkxDsXWcbCCoASqQIAYDRmDKr6sPQJo6wQlIrckGa93FyZYt9jVfHiiK8elxnh5DeKj9zC44oSr6vFy9bWUPl5r0yi2AK8f6ofvJKtheE5ltgEYMIXALjflCOZO49IZCFm2zOQsXHrSBHmWle1OIU15yWwhW46kIKUg8SUJAW28I1%2F2n4%2BilBJXAdOo4nmuLonfyGD7BRwnw8L4ZY%2F5kdbjN1Qd3VFVExHWqJxCQPNl6ROT2rLSd1W%2BZ2pNCJhXCc2lL7YRVbJBkup%2Br9e9kq9jgMPi%2F2cYGOqUB0Bz4CEXqHJN%2BiReh%2FmEAi%2BMno8juvRW0UK49sMRpXFK7d4cO3XF2oIg10YO8ctlHw3i3HA%2BC4wcYX3IvEqyzRSHSC4NBdK6%2BqLFZejLowOzsW5YPNB4NywfZ5npS0YvrXId9wwaqBYBchfkKUHUINsOsEufg6ruTgIMFPwaWBKXsQZT%2FSeYfbRISWHi4vNcyK3o9oERqsnlKPaL%2FIr1l3hFt%2F%2FAm&X-Amz-Signature=0ea1aa7cfdab47962ed28cf72e53022c2f5b5d9e3597f0d39764e32acd2871a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
