---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCLNUJYM%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQD0VAze7xofzPAbbvyK%2FS7L%2BJCCaddzzgccEMOI9Gz6WgIgQqtzKQhjPL9TpbPGJ3OqGOVkDzE3xhfmMuWgleVPRLYq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDGZ55Y%2F7656ZTASo2yrcA5L15IA7cdv%2F%2FDXXGHxsioyRSo1Oj227178qJmN4SeqqOqvcZZ%2Fv8rGM20qIC6LPstCELdJYNSDpmGV%2B7Gl5kqcBR8lI%2FJR1uABo4lCR1UzdNbpPPICkAEx1Ip5lkBN5BwXmL1q9mDxnbmAVUa8jRtAaMV9IK%2BYMzg8deED5%2BcABxL7oRz%2BqQtknJTEOCUr3xstX%2F%2F%2FSWpS0xypNFPdSiDrrH9%2BsSUJyKWMnscUV%2BpdknNkfFnG%2FXCHafo6VlJBcBN0iBIcKzpF9IuNAynrJyJenvIGK4w54VsapUR7JRJwRGEh%2BLwUD5oP3nDvHUolKhqRvG4BUTkjSbbryqqYt3agatCMSnnNaDeV3S1Qws8xRDSZARyfdmQS1DEyGJ2CECG5RLyYdDLhzixKmDiwFhQfrwuaIW5AhOMXz8nD0IAdduRK696uCRxjRi%2FiVoc8gNFJMZtBgkJWiVJR1MT%2FtS6SiulBrfoFCXQToHBNiCBjQAaQxpfsUH89lnJXbdo9j4mXuqMokW5DoFfyYSV%2FbS%2FDNteRY6DaU4Pg31E5BZJQSAOZwGRd%2BGf45G7BCHuSHEaP2AlyWbnlDMr%2Bp8ECjj%2FXF96jNiOjoUk2CqY5vGpftDvOWWYpK01FvrK3cMO6xgckGOqUBypUzSRS%2Fxh9YS9dBVTAOUPAlkadKL%2FhinJFZ4A6ONYr0taTWIAUDdsjnFkSaKWdTxMMBsQaCGJZHQbpf1S6WFCI82jHgSuaGHKG923LXU26Hi0vu30xkBwY8q%2FCDgcg%2F1ConrZjT2axTOWDUyZZLpwIPntyPiowiYTFuQKH%2FtKVook1lk7t%2BdDy3o4xQL%2FdAmKGzd9MzBf79nlWDafeZSxM4hEUW&X-Amz-Signature=25950ed8b036530e112184df2a18998357a52e7bbe23b0d297ac36d706d97bbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
