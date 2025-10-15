---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPUOV4D5%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC5xL6r6bYPCvAU5lK5a%2BnW79smD4JG4KMzmumhdkgV6AiB6eCRT1JaWj6MSiDj1vo9JKUTeP5%2BGECsSh6o7gWNtxSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMEqQtv8vsScVKYgHBKtwDGejDMgKuRZJU6caOtDtEcheY61csW8is86Ryh8TbkINDHbaxw%2FuP1UEvXbAl7XCIIuKoLpVRYk%2BQW%2BSf4fDBPrCaU8h2Q1ts4kqGV2cVsZPQ9kShkx%2B0f%2B7qiXlPodO2vbjXY18Nl8qlLtt%2Bw77wXVJYNi3dWTxTLJ2NVMzb5k9MgEda%2BXlm9VxhX0RFsFS86%2F%2Frr7jZGWs6vnPmL2yESTZBwxJFpRB6AxVhg0UNy4B7aiLoQ%2BO%2Bkeb%2FQDxeGHwz9%2FAYISPWk4loakzBADfEScGmaOP0xgEgAkREJnlbQ4VoNfdJUkiMydmTGflOJsnDi7hY9qE3VUb1YbmWJO55j5%2FWvy7eS9tnHod1l2zgDeXVRyWMoVOs1qNtmZ6h51Vibe4p4E5EduR7huCGZQ0xOO%2BI0bY2fMhu8zP25Y5DH29TCgeqH2LOBZBbqXI2qgBUddgv%2BYx7EvNvOKkJuF1Rs3SAM9wTEbEjirXn8KbWmmzUSYX%2BZ0wuTs3fIpS9N8sRmyw1qDM%2FHe0vMR3NyQrGYs2ClQPhlXdZk14swnzQMAnLW69lh9Btm%2BndphEmV0zCl5spnNbH%2BR377v9hJsj3sHb0Z4urPPgUhvJ%2B2TrrKZEGLtjvOBOqrLCkSnswi8S7xwY6pgHvCd9LCSk7Da6dpBZQxwzbF6QbIn1WQwsK21bFSqMTHxtDQwk9kSqN9h%2FCw6JPa%2F7vugcx%2FbGVYijqttLpz9rKTKxhAvje64HxWkwhvKU8qmrMoUhzzb3NS9%2B6rBz2wlC2XIKzWeTp01Cr0wrEGylZND1pfRo4WCjsuESo91VPR1d4Jy23g1sM0EvX6ccHGGXQShe5HgfI04mZX6gRHdSZhmoKpKxL&X-Amz-Signature=de8ca50ed5b19982d7293a67d78754995ed59ce881b953821abb3716b409341f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
