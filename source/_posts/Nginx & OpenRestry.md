---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3P3N2YX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJGMEQCIDFBxBnRBlIb%2Fa0dAjevNHImZQzGpzn7aF1385Sl8khlAiBNdFPQ25XIwtGi28YMUeAjEc6z3%2BdUY0xSznr722E6zSr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM9gAn79qFL0RCcYvaKtwDuCoPM3uCD6fxQ1qM7oUOQCIdMfqNqzWKejvw6g%2B2gUd5k3t3SvMbZ9P4jr12rrMUQx%2Fp%2FVr8MLV%2FT7LEp35Fngs58Q7djSkpwcFbgvwFimzBpngvxlvKUFehNiA19q1VTstEGgdgyxLYh8GuiH2JwHXLiCMytKFiphlWnz1IOMAtXwknl%2FvCUzHokXRAXCXFBF%2B0IXmkB0yDqnVfOtVhgkNKMPQXExbREjlo5jnd6V4qxprzD0sqOjfF8vYO5uM%2Fbi2liAzP6o8JrkZ48m5lFcFj57uPalwyzSNWvxz2HtDAuMAadGT%2BZrWaebzzy3Y5XUOnDPCkvG%2FprjXdbi4fOJefGbx8Tiz3exsnJIPC%2FPhCqdTWSUvYk5DtYhKSKcsIvvmIK2gNTRtQk9p19raLCdn%2BnDkRLhr0PNalOoZkF4vgo7P9jmbn0xtAlGvf7D0%2B7oGg7jlZQYQeR2F%2FlDfi29YI7kZuoqa11UL4oyNClyBO%2B7wtGIkcDO7Ct7%2B3UBs8AbA65YlV%2BfhCZiLbOFyp%2Bfxmds8pL3I3YDQGIk8lFQkmXgq5VAlgBdstGydbJZCyrn3hQ%2BxZIQ23HdcP3xRHp2tvfvXZ7BejgJH%2FgVnKy9l6owODKczDU8XRedsw%2F6SpxwY6pgHq5mIes30FAuVkrQFnknBo8azKan4c85JwHsguCMeEQ2vOhrIrxp1Kju0%2FxhVlqat9OT9nYXX%2Fj60uTIaUwhll3bow1YiKmm%2F9gWa3apSyO2s02W0MZ6lR8soi3kSJddZkxBPo3KeorHVXJq4Zx1%2B1u04R22KqqCeLB9e23PU%2FFM18ZIJNc5mtfBgfB4DEBxI0TO740Lh4ZcN%2BfI4CKq10dNboO87T&X-Amz-Signature=e6b61bf12efc427da80af400c4ab95ffb51ade74323a4b51e151e65f42b61715&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
