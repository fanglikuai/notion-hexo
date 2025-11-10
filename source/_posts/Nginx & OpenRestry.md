---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662EE52REB%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJGMEQCIFaErhx4Yf8nZrhUtoFHtxdiq58TTDkEVlRjrvzEu9JGAiBQ4NC5w6Px9Qi7Y43VskLMtH12w1gHA16IGdRtazMQECqIBAj3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3T83upY0x9fYejfNKtwDh5X1bkJkkTa419Ckkvp97orXW2lPLAhsb%2BVraM1muWsFRpLiEzDLOnsd%2FGWoP14QOoWzW8kVJ8nEQ2ZeYKMCeLNPzDd7h3M8Y8zhjVfE9aBUjytGMoYgvmR%2FoHVa1%2F2KCQwZUTVpD7LFMx3sQ7uPfMyYnM2zvxO5z050sB8M6l9FP4MdJrqTZp2h3nUieX1zoCHEzw5YB1dxrd%2BkkNIH2KZUXD8xZWaLlVEZ8OmEzZtgQiKlBzaptYSL%2F4a%2BIbAX4HtjSNsrK3rs1YjJ%2FeEJCn0H8YC9LQIhkigupAmFaF%2BBOmdSuCgIgqAHt7gSMMhxO919cK38sbOyri%2FmlN3Lv3HCZ32eAd69V4I5kSTLQi5pdvWSZBoyVyF4TsxRvFOPtVw78FyufYgOInQL7WES0oGW127rc1K7zpD9Chg3Ixq0TyxnO3%2FCYlsLNJ5tyL0jRgRhTkEBidf0DMZxkZwrXSzhKP0bJwXQcAMcfQD8VlYefCwXlh6aEcy%2BSYqSKKm%2BW1uTFC1fICS58h0OoSiPEiAuBJU9%2BZaPrsW5P92U8o8WvDv7GZx8yaLUuGsO2KgOHtM0erAdT897xSn6s3GOTYt5d6TRaG2LpQbzNCCUTdvfIULQnzE4PgrIIWswmaDEyAY6pgHLOjmcRKEzY5Z%2B6MqTB7MLtmzUpt%2FZdu3a9Z00ODUjFQ%2FZAEGfAVNzmA0%2FxiYqFwZll08uFKSGz0asu6fDhr%2FoujFMzKpOOvqSqqP9MnceWL6l7Voy88LaP4jH2UFfL%2F3RK%2FUrW7EU%2Bjuk%2BgMJXwTYk1Rh%2BfasCM63z4Bss3d2Aign3VZh57jWQN3HBvgxKzE4rC2h%2BWYEKSdtRTpTt18R4BmTZ8Uj&X-Amz-Signature=bf7c5ae444934a08d4545aea6a0e49cc43ccb6d5887cefc6dc132b7c08e6ffba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
