---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YU7IWCAG%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T080723Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDZh9HLEX2ksSHnFshVtidMn4phb%2FVcVDIDDiWA3WAwcQIhAJYkYmx8KAz%2BBJA0IR53u5NC8XQKdrDaoqL%2BKl%2FhFMzEKv8DCG8QABoMNjM3NDIzMTgzODA1IgwDTwb8PILZ6oPK5foq3AMIBRoqMqWfbq4mfeGeB2iNsZAQ5jTuZAkAD5GZZ%2FJN%2FTBFWgIhgrbbRXIOIb%2BQRzQsabc81wzVTFx%2BOqeGOTwyDcGmQEG9b%2F6FpB3al6K7Yj13fZbRMa311YeAYmcLvWUx3zr4D1HTDCoy%2FUslzrmu5YN7WKZaDySu6q%2BZsyYwneM4SAUk31dKx7TU%2Bx9B3%2FaOQGFq1x73Nat49HQahHdlXlB8iBp0Wzecv%2BXlCCEn2Ra1bFepXTPozBC1S9X0038IG8ZZlzzjfIblvKbebdxlVHVpI3ALDcDo6LaGwe%2Bd4pcRlsbIXOvhBvJl%2B9xQK04ioHeDQx3QUegCgjETBT03m%2Bvs%2FPdMXEPLKfvoXEp4Ez2ZY27bZLY5JCCh%2BP1BYy1YZQJ0CS8XDAXFwTlXDRdhrYx8pUxK8eAQwhUf%2FUj8%2BUNtAS%2By6LHZJX%2F3EFZ1tmMcTXt5T25kmm0v96pVQ5mAew9AwPhUQdOGjQrcOczfH6X%2BTnYn5VE7SH3cQQnTLwTeZvHvOdmJLs8hFybfMBEQpBxB6%2B2x8uToRwELgZ2wvc2vKVn1Q%2F7ymRJOhE2C85aYqz%2B48BiW4dau0ORPMgX%2Fyy5aI0SZ%2FESIS5V6FRVGxPdSJsftkIsSuGFtYTDc357GBjqkAZ3yvqVQyIeguvtSKlWEPiop%2F0p1205qMD4frxbdpiDZNyu2X2Pbar8n1BU5rj7Ag4q8ExrGfiW0IlZVfgqtEDfiCv5osqfYHHzJ2Xrc8XuM1sRKDgrFf1sYbFh6MReTvM3OHt5w9zS6jvWvBxmhXPGCKWiq1QckpLtwN8pcOnVevGM%2BtvgFXz5Mu31yw3p%2FhvGBE7YMQ2FK0%2B4xvMeX6hOW1veK&X-Amz-Signature=c55726afbfd1e11081c862ea1083dfa4a519bf74b3b46af2de2c27a49d2e5593&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
