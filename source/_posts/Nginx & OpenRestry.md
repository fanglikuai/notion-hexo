---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SW5QCB5%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDoz9aWvnLCP2e3eqbz6agMVyQz0%2FA3%2Bk6EF0sZMv3hXwIgSnUcczFfEG%2F9xySepsQcHyBzZ9F2cx%2F3nXcBFlEp7m8qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPcDC0v84M48dQBmKircAxijXnJKBrPn6KUpl4r1eZ%2FWzcK%2FPJh2KvzhwJKOqb6gFARuHLDNrRxf7MFfuCynBflD1H7o1sGRsGQiTNVB2T3e%2FTr1VahZNTuHaTIqbey7vOWiWB5waZ7VM6%2F4Yd9Kur%2FY4FuSAu5W%2FaXTYWDfWxds%2F%2BHnWDHDcXw%2FjknKm0peXhzpyZUpVYJHEJ9s3BMrA13ouZ09aRhouXT5p7niQXZNR5Pfiv4qtr07ir2OBeNRqUAKNrQyp3MZNc3cqzf2Sl9i3fVR51ZzD8HfZOMeauXXlo4vX8rWC1rihrpjGG7dZD%2ButawgDygOvGO0zmnFC9lZZgi2AmnhFdqnaXp4o6iuiFZQcxDadiQtPs33DNED4f6w6cPNxix2esq00cS5PBEiLYgtMAgk7xVOo15k8Z9u%2FEhNPFOltwK1DMoXEmIDFeMV0cuvfaQadoqCPgiyhbfcG03MSrDgoVUCGgnoKafwPWooWtP5uMoU3mijRYhInTilA9J7tKx2fPzpVqgnzpd6NaoINaTrYAYwhCwqfF4AO%2B1DbxhvwKYb7Q2ncFbRQE36C1nGgNZquxnvMhHpyOl2IGc4F%2BuozQGEs2mjDq4%2Fbipfk0fuBdm56vMasIA%2BblXqeVvSD1tGFzfMMOKYrcYGOqUB%2FUA8mTBtd6muXL7UooEJE4r12BFKTeUhFJzMY%2Fae%2BaoeacRLQ3ER7ZEZDk%2FF47ZcaGBeWwxEIVhsVH9qx2cSh4E0eRSaJnw1g08t6VZE9KDRQte2de9OR5jij266Eq4FugxYt%2FqIDOKWzTwCfhG4RCaNe7WIweIbQuIevP1iRHuusUJcuruLAxNwQsjC%2BPC6JVXOs8dgGO%2BiTt5kKDjp3F%2BlTKHo&X-Amz-Signature=796a211dd254492b8e3d18888d1085ac7c082350187a2691346288eed9fe4c7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
