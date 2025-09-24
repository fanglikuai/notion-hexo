---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Z6OJJD5%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBcbEbUvw6pGyxiEDLL8z7xjGUQ9Ze9Gj6bzzoQCUTjZAiEAo40uoe%2FRsiF98lr%2FymUZw5KsQPSVxYjVtQ5XdYFPFi0q%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDHI2sE3PoBJ%2Bjf229yrcA35A2YTk%2BIWNFtnPuMzZJ2RKlawuCbtWQjojf%2BsE5wNazKUHWaOVfPBIGml67IbsxfYbcmZBjYQU%2F6m1jRy9YKf%2FX2SxWif7CDjangoCKYB5WEdhi7mK%2FBmFSvw8VtOnoP%2F%2Bw6%2Fho3qox8OncUdsoXVHlbN8B7mIPgUQdwwQePKEHYX06eGCEvOD035ItJK1MLG6l2H31hJQHQsWkc0fV7z%2B1mHwOADSono66i8UvAWuso52crDUTPeA1owG9mIGzc3DPOvWsCiOS%2FSN1GEnLzJCAK2IjBK7rIAsEN4K1lA%2FXur0o7OJ8k8RnpJZhsYuxsYIoxO7lFp20atiZxHwEnUzRwQ7TRHojvHO%2FjiE5dmWdZFA04q2PaAHLdHlRlaBYGSIhrZKNaHVq1k06VjqotM11KmVGYZaVHCcK3Khwfz%2BdP0GOwOh9xRrGzU6IE2Qr8P5NDJx4BuJBedPbqkzwxBdReQYlfUmP%2FO170%2FfiHAr4Tv8oBZbTFNEVGq1M%2FARsQl5COBjoEz6OPRZl4nRX4yQXQ64jj1EVmftT8l9iETO%2FrbuY%2FsgRX88TCCDTn%2FoexY%2BfHEtmMakYwxTSw9tdBIGlpKijT63pkgF3Co5na5N5sCqNZqA5%2FeDeRvFMMTNzcYGOqUB7%2FGXl5dx%2B5EayqPSRLCvgopJf3Xe4VtUPzNtkBLAT4nONe4QyyoOgmWfyQ3bDHAFv0NNhJ1yDIeUtpGz9ldq3URkdwbcC7Yvn3XuC4vMKxracLSYz9lMgXc6rjkl1nWdVtww1sbfdLQw48T%2F9nMFsDxhdGaM9ogQFZ7H9deY%2FkSVaQAJBcwQKnkWyJtXjBMjvQtPJsK%2Fm9LJd0rhKHFxrWqG3E8S&X-Amz-Signature=0487cb2f1fdda41a0b6c386474bcc616084944c76d454f8ed04f07d7151950ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
