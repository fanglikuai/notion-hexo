---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652KTZECL%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQDoyzbYPDW8MtIHoOb%2FYWp84rJ5INwEULFaMuxu6wy0wQIgMn81sSH88qAcuZcFIpL4xP9kZB08z07eODycC6AfGNIqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQM0sN%2BmlD6lf2NHircA6enOwQ2DxhHmdCCo0gTIVRfNHK1OeOxNscqW8vdpGx9x1BRtHjNINBR6hqI8pj4b6effG%2BTMZK0KIaVtzgG0Xy71Qc%2F0AShQ9iWTr2gpS6EmhsG6ZiYFfMX3mbN41ML7hjieLZR8WwzC%2FC3WpKFeJ4xICe%2FDpdBUSnt498eCJsO4Z1SNODzwIel78l2t4XzlvUAQMkXGeS8BJwjNBGil45c%2B7ZShy9BTXMkzlHVvovgNDeHHzTMKiSDqkxgYpOailDOH1LQ%2FMs79lJAWTRVLtzeSm%2F%2FhWIuhVj2CX6rQys5tcwVJS0i7sWAJXgIJXeoF%2Bq%2FRfeQVSbqJiAUDilrT8ovdtH%2F2sHY5KltSHi5XmNqK7ILAGNau5Psdf5S6NGxh38d2wSDIyKm4BlajqBpBTJa5Xh2HKqN5tL7ONe689j0Ao%2BYaBRR6OnWGMshw4bmGoCRpodSncKJGgUpqDTNe%2Bx%2FbtqzLH0jf3hAdl83HK2pVqTPjun%2BoxxK1unDcNhuOv8jHgjJp9COw2CMkZzFrhM2av4uhoRA%2B5mSMgNdJklxvKzO2tkIQp9tHqL2nuzPFcUzbz5sLgtnXXVzhsrBdFhLwkq7oXFWH%2BtoDqFydRjC%2FYtdxyDvv87QnRp3MJez3MYGOqUBGKDcehQYHTSqUzgp%2BCErlhkfpmi2K901Sl85oPQolQuJCN14xZ5CK5BZklTUmrEbV1pqS%2FXNvUta%2FetjMcAPGu%2BoKetPUB6A0qkoYUutg7AbAezxW9nZMvprJ%2FvWycV%2BZHGwckcc8d%2BX3m5JYxh3gpsdA54taScQIysUrt%2FIopTr%2FTGlzuWwqqzOntrkEBgK8GmNMeU7yFMBkMpcn%2BkTjiNPHlrK&X-Amz-Signature=e84e0b13ac9c3256a6c6fc7fb728bf9d9c0edaefb07123d82dfb567839e44317&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
