---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2V7VOLQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQCQB4GQ72634WcbM7VURxyLuDhaujhS%2FpXTEgYb6BuG0AIgYgmVvizYgM3surNmv6018dQ6x6IGWI48AD9FRRig6eMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBGwaWUwaMZANYkEdyrcAwJUnEMe485HiuIg8opDRlE2ZoNrfYz6pzU%2BsW9mp01R1qOE3xLY4WCdqugb0zig0wKQk1V5qt9Gv23qh6UwdIYcOUrDwHeKrLDIQnpRYNbabVPF4favNn92dXTy0pV5gb782ISxq6cNsC0d3Tus52qx8eXSlzivwgEWN%2BA2H2hi%2B%2Bl6AA%2B1R0Fgf6cORs3efHv34hQupe%2BR5%2B7UpPBrbwyDLD%2Bvtk12dpD48k9TAeQo8ENhsx4RWeamzfRnknV%2BoVHi65f8oivhXpgEn%2B3xt6SLd%2BJsWFRHae2goWuPZMiHvwRNvzoNuFz4IMpOC29eyTdznJO7oExiBmGxnnGzdGpJPeAtuPkFfhx7%2BqipZxKO%2BBS3m5%2FBHfNzIFa3fee%2FIq6o6Y%2BzZhYGTAOjDZzAJWJX5Wjgg9RzSsYIwLjWGd1pOMGtCOGvkLNi667px52CNQcePATWHAwd5XKvZ9RuI0qjhyx5aTGvj4Y%2F8CMX3OA9Gc2Soqfn5Pkv3ypBKt%2BJA1L4Wg0bMMaBODJflfuh%2FS9ayR9LmgZFtxd6m2LdYX5kNxwWUVNIs5D5oZ0ggRMBUvrPqAUG%2FygSszsClXvhU32Gz8kz4%2BBJh1y3bwLYdIHbWBXNIz2sD1OHqF7AMNaKtsYGOqUBoy2LVxhYGq5dWtF2klvyIFSuQ7F8b4Jgpu%2FYLfvWawuvKe0u%2Frxmit6tgq4XoGMoAPVYkG56BUQ0tAVRI%2Bu2k8vwBMMlYCnfvufniJHMvsi9rvOmjwZ7OSkHdYaP488LPBqnWaOH0OqsY5ckJMlxCDmWrLEqNSpBnyOS0cQLKqEi8AxkxRa552CdFzZly3nDV1qhhiDfw9jvx7K6xMGsMJqXUvmX&X-Amz-Signature=460f83f0a0b35002e6692523780cc7b7bd7b353e2b6d310e51a298d67798152c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
