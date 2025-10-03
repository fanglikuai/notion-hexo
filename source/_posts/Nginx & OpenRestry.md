---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6A6ESP7%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNQu4T7sufa14wiLhBugW7vVBLP9iaejwNTkMOc9X15QIgVWSaBylFLrFu%2Fj9k7YiTjzvAJ9B%2Fx0h%2FY2UHmIUFYAAq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDO9KhYTTPNexRPG3ZCrcA3Og1UJUw6sbUoHxVIy6FABVwvjRoQ6spa5c38za%2Bvy7mYms8%2FO3pj%2BW1u8GsHokRwWhZnFd8LH1sgYygdomFJptdSoEMGC8TWeBaDnkQS7fxBfpechBhij%2Bf%2B3sLf4E2zA36%2BKcF%2Bw9gWJ7PQLK8spmBsiBBFV2U6qhiWY8FCoxAHsVl6%2BbCD%2FCaY9mPLuW5CYncHO6jKnPTP66VJvBzdKxb40%2FvEAqiUwnyjlNDIVr5mSiUBwovNStC%2F6XS6kgbqPLU6JzV9CTUyyFij1AIkSjkLgBiPBz94CTq95SrKIzDzLHivhuIfXXtponcIkF%2FAUhehpwc%2FEjUqdLgIIBSFCNpDoN0m3L02mAlJGRetJcxaLRUqWJ%2BGdQk0z6F3wOOGSiENtJ8AFS7lMqwHoQsxjo9GqNQwuPalMm6EibnsOVBx9ySW896A8bvD74vSIqOSr7Iwa8IJGWf5uoCf2zJCh4MAqxefvJjIdyqIwJeemUwhki6eEyrrwE5Sur6WHA4hpju%2FCA2HGQ6CU39%2FbKle%2BB5Es%2FIrGCfIiTTI2J7N%2FKvx1nc5pScF%2BxYJMEQyaR0Orbww4diTgGXv78fd5h77gMKc83rLwazAjKJRg66e7rGvy1Oi6mh9NkRw0BMKGNgccGOqUBdGMuwHOhnmi%2Fq8mrY7u3JzmS%2B%2FjgTpF5fiD46%2FVEypCHsT%2B%2FLT5q1qRusAhuDRynKhnoyZNGB0Uc%2FyNNOENtGmGdwLBpe4hsADQ%2Fm59lsyB9zuaQG0lB3azpE5f75Ul2p4SzivK0yi39PcgOPk6BUxllPO0dn2OFpEOdBZj8%2BIemQu%2BGavYS4drNmjVdq%2BkfryiGjPdOa%2B0VMR8ethyfHPr6MOI7&X-Amz-Signature=f44d92bcc997db64b563a2bd1d8df09d444e2aba3d1fcf89fd00b2e722b8189f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
