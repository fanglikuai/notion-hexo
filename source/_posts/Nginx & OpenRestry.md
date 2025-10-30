---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RKOEURD3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T140112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCXxvqmmLlOdzlbQOQ3iS40qNG25pSEjT7X5FeEvki%2FnQIhAIx293MsNEHbAUMN8%2BbqhPQv07G4Be7u4FGxeuim0ewJKogECO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnaEc9OKwEEEPb9VEq3ANq%2BJObhNLa7g2TbgyeZmRKHedQl%2BkUfDQODy%2BYuYd%2BqYbCCyJ78SMUKyOy7xhb1y5J29CJIGbXcvWNpYKSRz8L%2Bf4yxG%2BUzuK1J4Lar%2F2GVE0ffODeDCXXLi%2BaKy5xxugj421tbYhpvqjbU22gq0ZVcIiSYTwKGbV10AlJc6q1ggx82qpt6Sj6%2FwzOQKmB3nNubctFaLd%2B25pp6b6Ad8R%2FjdyMZZFvv9wanaxU9D%2Bz9r5%2F1QIGYm2aKYLzoPk7ECi9t%2FMdckNnfJRUmPMHlptBeBRudsutrofRnf%2BxG3ProGdvSwkQI4wMT6yqhCYpnkMNGTcXsYPeibgwvt6%2BDGZd%2FQeI6TyN3OVJhVyuJZWnzHFkb%2BDJ4m836u5DqoiJGeEdtYUGZ0pKHL%2Ff6CrgcT8d2nlXh3JINU84Do7EqPdCi3T2RkG4X8Cp%2F9gLFUWLanab86OQXhqtxb2H6dOkElrbSxmbvQtxbHc1E%2FiyID9cE7foft1gf7CxHqGD2awsi%2Buo%2BvorzcbCD18CxJW4fWo7Wqsld8U3QborwVQ2yNe0zVKYRRQ1tUlBDIKteb5Gpd0loafnhcI921NhvJbZnBeva0nXt4jR3MrQHsjkNen4LjESp5AwtL9tfZekuDCs2Y3IBjqkAY3NWIgIHJbCtjkfreOHM52NRoRMxteYTxPRrseEF1YJXunCzkrFdh5zjFcuWSzyS6%2BIeKM2Uisn2SjcCK2S07bGntrqvdDewGJbOSDF1KafKpdifdldxSeCIs%2FQ%2F%2BRSwyErkGVeifayD3f11MKb4wWsa6LG46IGN0xlMESvTqkC9Qj%2FJhgMo6VDq0tcdl24VFiESfoB1sxRMgfA6s9XXi55STdN&X-Amz-Signature=0cb2906d9e53af00c37cd7014852a71344f0d10c4b96c2c5ab1e3d302c7836f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
