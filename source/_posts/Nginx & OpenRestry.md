---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GAWDISB%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGX981WJGmgaxQizrYeP8AvhMrjVPTGAfHpB3jeEd8eQAiEA8D003GW5Yr1mQpeXPpPaJmLjIuBKxtoOozojGAtNYe4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKaoTaBw6qF1djd8JyrcA%2BkmXkYS%2BvNMnNMli%2BwtCE66zhEFOYG4RoJxZS%2BAbzCuJqITcggMbEAuIAr4ZivYmpBQuGLTtTs1VU1BCrcMTqFJTdzrYRc6gQuRNXeZt%2BWOdb4O%2By3fn2I0a7DnNbcoCXD4CuVzKFJYXPRouo7FKWoMQ33fqGkLdys4%2BXJ44wf%2FQdyXguUmPAcpXtdlneAfw4mI9ScsGlAY1K6al%2FlrykVYKkIg911gblpmiPS0lq%2FTGfxh8HJiSGspwJsogxqOoErpvv4hjdR20UR3%2Btms4CcCSAi9W73%2BQrey4OgJaWHcNgxB1kCNZ1u1qis6VX93uEnpqF%2BJAq0QGjWCIAI6DowDoBZOvpDPJU%2BnfkZPwSZwv1ch0fPDYq1Lw6J3CKIzustxTkO5PamJa6z0dB0a1EcJ0sh8Dll50bcASZ0EIwWgRnit0WG7%2FxcgxHn%2FkVVf2MirUh5HYHz0GkB5nXMV%2BQn7Stl4o4sJ5XLOseCO3U9%2BsSRl5PDBaHYUuHFoTZNy40oumz2MaWhBMNAH8LUtFE4dRRrNkA7TYMYftJliKR3BE0yNl4G002uHW3L4e1Tp%2B2J%2BczJJTqJstmIgSf7Eo9KTRjXDIzAPV50WYffnF%2FBij2wZB%2BKLHX1KFzATMPftpsgGOqUBKUIST%2Fcd7EVCCgbc2Oiiiy0R%2Fu2Y2zMJQNiWNdveDusTP%2FfpvF4Fj9gT9rjyxbtmidub6XWhic60mZnKSAnV05ZitNY766RUdoPh84tCOdZTqgLsaf%2FAzGH9jZ6FY8yvhIN7F%2Bo4XJ%2BjPj0ys5j0yPXJrUaiP%2B4OEx7DRUyj0P9HtWMNJ7DBrnZYmZtTfy1C7yB9dGWy8bhgaRjPeadfexs5VZRL&X-Amz-Signature=3836a252fe240cf2657ec193b433721cd6fae15ae5a67ae80440ba5361289bf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
