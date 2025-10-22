---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667I64WFPZ%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T200050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIERG89VlIm%2FbUgFHj3trPdngItM821Q%2Bhn1mE%2FmOXEW7AiEA%2BtRIph3iAIFs9cwQf2w3w%2BpHJ5Do7gxKK%2Bk5aV34cIwq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDPesoEnMtWc1RdawXyrcA3O26%2FCOqbET4qEf52aFkydXrIsN2Nykbz%2BR%2BIOcNMBi2TNZzcl0o1irp0GECUnN7zt1GwGm3V4kWAh9sKZT5XO5ep3xCeOPVZGIFNAGomSCI8BYp2iPP0Umjwj47u0JhMkBYM1P5QD4XDQdbdTofc3OnRAH%2BJpVF4eVy6cvzZzLLR5gRRjF01O5Rw%2FU%2BP0UtTilJ0oILYRMMwT15ngh8YYm6nLga%2BpM44IA3BYLrG9pCbCelN2Ro1ZqZ6OkhtA07ghqX5TRAEg8A4G5UwKqiZUnMU%2Bj7B8eocjikyp17kbV5SJhndcBzxuQddZRKWRzJ0VlPwcM2NAv0UUSSTVvnCXi4QNiobuRtzMxGTn7OV%2FM1dcaBe0YaYmEYQnIpGVvwU6WVWU8f6BsuEQXslPlk%2FfmchL1%2BTOPXSBz%2B%2BsaDU%2BYA2iWJ3O1f9dCd5siwZ6LkDavjqYgDqNv%2FFj9ur%2BImzKN5WuZmV3OMpWvN3lQSQSxUyTsiM93rV1J7Bh%2BCezNueAI%2BADbOFeTjk%2Bbfp2jna01%2BbaV1%2BFn9yc8FQRWjB%2F4xNcTibwxyeV3xGVM2nqpfADGyP6OihhcLbA51xoQUteoMJxlebKHv6hBzJ7XrKIXK9DoQ721fz69hljhMJq35McGOqUBjyYnkJukGB26JnnhenvkUj%2B8zNb4538tRBQl5fFoPgTzz0RfkXUChAFIZmRFAJVYrzCQ6jIFRpLb%2FxlgdaDnwgHm%2Bp29mezgMhohLm8uT2%2BOX5HQ4IZJwp%2FZof4ujOnb4uTk2sE%2FVJIWFO%2FEhlKcaN84W1SE6dEV5iXfHmznRXTrDhzGZtDhkMfrxCVT9pyqWlMWt1Uq9sJoG%2FG8DHoj5S5OVouO&X-Amz-Signature=8a36dd7c10b542bbd4d50b5d7903da312489dd41c6664c1bf940ce017f0a31e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
