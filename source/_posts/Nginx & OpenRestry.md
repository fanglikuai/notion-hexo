---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MUZQGHQ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBjgCp84ZQ4AkqnHFAZdw2QBCdbkazOPuLtg2uIOzMQwIhAMk3Qb8aFoh3jVsQ4NPlluNB7HTjJqynxhvB2m8FM16LKv8DCCwQABoMNjM3NDIzMTgzODA1Igxnyi8TpGvqwznrWdUq3AMYwD6O673v1Z9112R59JXzMtAsp%2BahtoM6GQvQBgLynEgRnOkMgFtGRLFv2pWCFsTkHuX4hh1Z9ScS8yD8YFE1ve%2F1Fe3esMBMMmvbydwjwT7%2BuRVyi%2BDc3s3Ou1Iw3KLZzeX4m8QV2toEBhyFNQm8K1aUeBLprJeUnUIZYZjBW1U3I8MW6icn%2FRKYTFHfB1JXaSobCn%2BeJuguRj8aUcVsJIl1xD%2F3r4nKWDL41o5KEBnQBxhjbT9dSLYZgiDwoON8JgUOz6KcheDMHNwcBGu0tDZzEQr%2Fg31moRW%2FR1pv5MucMPdHVaEZmg2KS9c7e0jBCL8OJ5SaFgahs7fMJ4IiOv0H9VTTS1AnxilXBan%2BMLLZ%2FzEC%2BSTh5Pa1ex57Ck9RD1twKXrYcB%2FF4tI31%2FF7Uky6U6k4S1QTDvXCKyNHy4QdgXRX3K6517TSjUfSo741kFMEk%2BC1TkihDsma7hyZ2py7ThyMqVFwu5Ego%2FbaLddiYoxntrYh351Y134%2FCfnDiGCZ51sWda3NhZo%2BKWoQu5ek6bsrMZceHL8O%2F7fYe6Lct76CoCTUu%2F%2BAbxkfTNuk2fByJfFyqTTpA4qvXa7dAs800Rag3ZMlk9IAvmm%2F5L3LNItvHUXqO79v4zCu0sTGBjqkAa5Xx%2BAgsFj0NI7CR8nED9FYtKg%2Fv92MYDP9H%2BB2Xb61sMTFz94t0f9wVbm3nzzhmx%2Fibp9cksyOQL3EiaUWy0U51Aenmzl48JsvQS5nMFcF0WbPPmCcC%2F3kzi7zUUzElmc%2F5YkTbzicV54HNxs0RAdw6WxLuwd36qNKYav1QEdQP2%2FYhk07SA%2Fqbimd82XKXQ9oofBE6emPXnyO8cfjn3k6tj20&X-Amz-Signature=af6c603ce85473a88820f5070b676a0e4c77dea40a2200aed0217e9a6313f3ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
