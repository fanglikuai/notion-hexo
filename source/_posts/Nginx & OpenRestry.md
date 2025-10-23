---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOKUCCG7%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBeYYIFC57l5lDpXO69qvKLNoYJ8sHzJ%2BqteR6%2FlE5DBAiEAy%2F4i8lNsHNqnK3VYsG9KSuodSVB9Ww1BI3GHrehC9BMq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDDLeTRFy2SZWhDR%2BmircA4io6zABaaOxlJKr3iVa186SntoEA1RQUMVviNHCIZmVoVtwu2%2B0SRS4cRM%2FsgHJxJOKdbwP6auTYGXAm3rELODzFc1IQE3Qv4%2Bfin%2B2fv4xo08P5ABzXfHJBArgG9uf2vEvsv4pX4m473C3eca9w9pTh67sYSNhKQtvN%2FTp4gutGMS398lySY3%2F0k3MVcdLlaCWTF%2BNohBCltT%2FnXjrjLigL9oCPu6klBA5Pg2O7V9WbA1S5EAHBSeTyHQdNDwVgmewTQQEY8ewez%2Bo%2BHYN0khit751Y8j1yVuNVgPpFsdluGmVOeU4%2BOB7fLO15DpgCARTexff9AkSgIkML55kpaGTlZw4T5HmnIIKsAsSYOpY4uRl8NY9Ugh7%2B9sakaudbZpEVLsi7oJIDgdbS7MmeJ6aRQJ9Ubg0Lx1P7KDkg0LAr1%2FxsC4BRgqaFi%2FnDC8FbOheXm7PMhsQoAP4KIffIY4omFPK6TXXlR3Fcq62uRNtw3qw1cCrZX3sfKak9IB2V%2BN3jdBmwn5bkjuwCx3AQUSn0n9xaxG45ArsgmKRiRKBZNBuSvjWl6wCsoeBj7JIS22u7NlUHcJkNM2g9yVE9kz90LDM4FvCWQ3kjULr1R6m1Mrj%2BKeG%2BeSbigALMJLj6ccGOqUBWD4WIUGVr7%2BXvFCDgwYwtx2MtDlGdJNbpfPjZT7L%2B8p%2FRfKSghnMVW6nRpJaAzu3vEmdip6GaGWf%2BWi4nqLctsqduUNZcuOIN3tkCFcvr4tgsR3hoamcZaf6q0xdSnA6KCaxLnET1zmv6ouC4rdvrDrWjrZGGFCvicA4NO1JfCOPNsQ0BZldzvjy2WucoFIA80%2BKUM8i%2FDP4ar%2FomPegqSn86d2P&X-Amz-Signature=0341e1b633e28ec0767148531d1acdec4006acb73902d41e6599ec6ff58b9b6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
