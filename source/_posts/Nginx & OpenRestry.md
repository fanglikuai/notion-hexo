---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2NDRGDW%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJGMEQCIFWICmlE7rvwrX2fYz7oBzwvKX%2BxiS5wf1n%2FNoknEUfjAiBQPr4VYhmCLPVvRWHD7WbGroNWYNM%2B8XaP7sMa9N%2BI0yr%2FAwhAEAAaDDYzNzQyMzE4MzgwNSIMpNUYlrpF7EflTXpfKtwDpfRYeNfnmlzvdnEhma1KJTxeIbXgWUGUmf92eyYMHhCYlpiFfUYbHuGeGojG5IqN2Vgp3krZ7jHpJ%2FKje22QyZpv8pDVr5GKl5yCK6fheCkGKtBGeUWaNxSwQ76B8EV2SoMfeeNcpXPejaFUMdMjLjijMik3WHCKzWpU0Mqzevpwyky9NaCirUZFBTPaohwl8jvBVwE%2F4kCiuGPzDdxxOOQpgK5jRrT3%2FV5T%2FXFZXujNXjWOUKGzk0arF%2BK6VWqVno43oBZuT9j0CPPUwJlcxxXvEbgc6P%2BlMPLopYBQMR1ozmoQ6T0E8%2BiDK%2BQghs0c1gggyPKsYy0RhPGLBsb51HMns4KA6HoDuDlJER2dgn56iLR7Wde9Bxp5peCLtaj%2F9bUWmHGYLUe2ng2pP8AuAHp8ZzZP5OVKvqrHYh%2BeQO79LYO9DJ66ZU83inT%2BzG%2BixIsY5TZ3tgv6f9e9nr7x0KHSX8FXAswc%2BUFXC4Gm%2FgpYPtOSGxRkxRlo%2BznrFJWAFv5CksXK4fzhZrgqEGtGA69mik6ZG2q%2BCZSTxQdhNiiI4yTemjjv4KDHzMhmli%2BudaafzuecZJCYipURC3aIwzXSoi1bWRy%2FDWOIrJo%2Fa01GPyHgcBqMto97QXgw%2FLqMyQY6pgG%2BEZuC9kSxkRXaQxoP1lhGk%2BcRBWdR8CWb%2B9W3ID7HMfHUhigOpvM4%2BJlk%2BR1DE6%2FC3zZtWkUex3cDOqZKOI%2FCiYU5MRNeQWPhJlcIrTdJZfa3hx1Za5ZlMF%2FWjSdzqAyqdSa12NHswYzc9HPiwlzGKrGH4qemp9eZHwBikFf5I2S2ORlhsxAQtsrMkCKyUtoH5iXd2LcEfuH1jpQyLi%2BKYe62fzF0&X-Amz-Signature=99d3b1dd7ff7932015a0670f9f83c9be3cdeebfd192b60fae83c60fff4c7faa9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
