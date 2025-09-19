---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYZZBE2U%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDorLTKu8oMQeN3LDhfA79K5IugEN9gnG7Y7v4qEmnvhAIgNrcF06huSbHB%2FboIfMSs%2FN3zj%2BauapWfLh3b5Rh5IsAqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOKplGqnq%2FT0oYsO3yrcA%2BrXnEDl5l%2BDXQLmwQrcwBNA0xQSuSIGWzGaAaWHiThCJOi1ypYzP7cQutJlCLe%2BqKLkDkG%2BaYpmojqGhEqBBIc2j19a4G4tkTC7nyiu8FT6q6jCYJdid5Ow5UawtxGwZUy%2BlHJ%2BwzTlOfczgvHyaEAZEUKMAOI3Zpx%2F1jVZ1ov5bV8a3UEMLh8xj1IdGqdc1Bmpug1qcA2X2e9HrZ5NybrogrWbXL8Ykz1hdGpFzPOAh8AGbJexukP%2BBQN58W4xajrLMrvQctwaqMDYL48p4vRBWqaMkK%2BTTbe3GvK9hvrDeBo44pnqddWD9ukLGe4ncmyPx6S1nOXntp2ZnmUWml3iLVYq5W66O8OD%2B5RucAOqmNY1THAHLdV0qrPiNzZXIxaaUHc3wZP8low3PRwT7cy7tAC2hc%2FZvZoIypfZYCl3pmkye5ecG1X836I9v%2BceRPy5YwrnQvL6sZ7MdrhfC70eX4x%2F3NzUXsxhj0Mx%2BTUdG%2FRo37FBu54xT%2F%2B5FdpnURNhtExGDJ9c5OsYykiU7lhKw1wXXFKfOiyTZ9MvxzCfehbKVJIvx4pkFDWDrUk8caojOVUZXPjYpRQ73r9KGOI09rMJCnDT9tTue9t0RTzkWtMvXOz5nz%2FsBls8MLWgssYGOqUBbzAcpgexknIerQs70q0qURoRqc%2F0Id8yMUSQFGbboIH38%2FylUwtj9uSJGiGpPF4ziv27cZIWBmGEJy75w7UQSV1fM44ca7W0QbbQSHHuZW200%2F%2Bb%2BKAXf7NPWwDV7y6LC24uxs8prA8Bc8J%2F5FDxXVlap6hhyEegUrQpJ%2FVmVZbhXzmp5Wwo1B4%2FibDDTngmOoWkxswOLXHbCsKm%2BYlVCFehRxa%2F&X-Amz-Signature=046f70d655d43624ccd9f2fa343b1617681982e5f4b2ffe7fc920bdd864cdedd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
