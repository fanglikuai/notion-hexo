---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BPU7FXB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T030234Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNiWTuOdNc3kmHRsvBcvTO8XBS6GAEsPRK12ykvEMbSQIgNJyIcrPiPt4C7%2BEIIDxIw3GqUK5GvdCnULdSPVTN00EqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ800jYwqoNzm3g7RircA7%2Fp0V0zAf4tZX8tpDzD29F2NRzPdY4GRS0icig2OJIKVZGR9bj3YANtf2mkAecAN2dpbgPqki2jOKgZgOHldzmF2zY7izPHM%2FK49ZttqaY%2FmMKPyLomRN8NI%2B9%2BbARWXXPUHVE1DtGXeOt691K45nsREUoeFFQ7GCyuR%2BN6yV1lM%2FZI0mcCmQRObjwXjudC46iwpMdvjDRHpQjEhMnZL3YyMr7h5gKB3Qf%2FldC1nmGwxvTYy%2F7K%2Fe72K0Ni51Rs%2BYHahO8tW%2B%2BcJRWTSOhsxM4FGK2AojzcQaghaOwixt0AreSbWm8zQV%2BVkeGZi2bckzpUT3Gm9NhI9dCz%2BoJPzkdEVdQKzdDv8Qxy5UAdtjRUZlmw0utJudGyexs%2FbLnW3pQfKzpLJ2ljl6zkkwPsJpf111eHOYAUpNZZih%2B0B7jBh5UlQneJCWZigtFdEX7wmjZWzhJpsgiV6y15gVp5QDYH8B8cDQfNaCGzbXF7wkVWLuqPuvEblo0g6%2BIceyvBmZKrQvYlOqyJrH12w0ligWjkNh0DdFMUG9VSs%2BCGZxtNOQBmFJ0TnGS8TWCapdit4b0GG9vlq0Pwx5lK%2BeN2cM2U%2FSbo0E1w46GKEw0cBAEKv2L1z4vRdbQtPrOXMLy%2BtcgGOqUBsuR2LKLvWB3Bxi9cIxQzOOHrpcceDBp3PoONK4zI6bzeVhFxFtOZbXUsitKsTDamq7gHoqM2W5LHrNQfrAwk8RIRxMpeAxVkb40Fn0M2xZFbeEC74HYQ7yWRRcU2LqJz2H3xc3PCV8PJ8ewJVS4mZ5sPVXlSlrWQeuEGxZ%2FXAr3JJtaCpPELN%2BTKDbRtTB5GtLbRQL3qj9dhJDfY2Zf5jC1p4IF8&X-Amz-Signature=256b7aa05675e5102a386d5b6eaa9311fc09fb3687ac9bb2017f15b978b57809&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
