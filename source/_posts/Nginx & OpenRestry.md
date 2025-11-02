---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7SEWTE2%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQDDKS6nHb2JwmdIskAprk4X02ntLta7zQGH85iT%2FeFUBAIgDFaQh2JAdcr7t6u%2FINVARwMPM75g9oycYZkDQDSx2A4q%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDPnPJRK1bIKc4LLxRSrcA%2FS65nFjSSvcHhydE0h3TAwmhLbDlQ3i8umltgrmcRsyQ5m4HfLJsl3wRECbrVWlw%2BCKpvW%2Ba4i3ov%2BZhiT9LTO4rmCaxAenO160RpAO0mQKm488H4nkDkWnmJ3Z3pS8Y4GaNRMfItxqBREn%2FxKmkozZGYpWtbiIkxRJ3iGkB%2F5WqpQnzk%2B0xtB77PJ8ipQF9Y7RqsLHa5Qh7stTcJLKZtRkjMqP6iWtqdxYmZ4Omm0CHDhKXOBXpsA3sELZWA8dOZ9syTHHpyKJxX2pBFaXDyTJTvFO3RR%2BsYzhjCW7%2Fq1fcG%2FIajH4q3I7g5an1CzWOoBa1EbjupMGyVfbz%2Bb5CPo2dLDvfXwJYQegzJpdTjg9sVdQ2l67BkBrXmhVgVEmw1TqlcO%2Ft4SOTw3XHOLys6izJVjtgigtq4Ptaa9Fvxqlp%2BK0%2FnE8s%2FeTNc9S5sDfos9wlzhwkJP6cDcWeFcgdSwZGesPz0o1%2BUZ7hg5kpNC3dYz%2F4Fu8kApnPsWF4LLTteHTxZlp35vEX%2FUZ%2B6Szh4w6tjnjBMzhVrZJLsKCMuFyqGPzF85tlWil1Jy1M7ELznDWQm3OG5D0ruXOxMWjkoez3awBOEdGrfBsqghrC%2FfPJxLjaVLk%2BgPyzh7LMKj6nMgGOqUBw%2F4jwMlHylYzJCBd8pIcEOO3n0OyzsR9sqlK%2BeYSaIyZ9ikp2PXuiWZHO7fXN%2FybE%2FvzpREUOgQ787kgVaoIUhU048rRJjDg963SDh6gxqylzUdFQ%2B6ZSHsduD6McLOj6qIiYetTwP%2FRG3fn%2ByPWy%2FMTKC6C4Cyu1RIn4LVuiUklIjc1zMYfq8MA0yimqYBoEdyjWEm%2Fy4YvzwvZgEYnDXRNcihm&X-Amz-Signature=7f8ffd69e7f6217e78242ba0e6a33c24d019ae83e2ef3d9eafb4b67ec7eab754&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
