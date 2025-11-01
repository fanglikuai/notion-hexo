---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XYPJIGRF%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQCpXhTE0GVr8sL%2BIwodqPHlIGzHF10zTEmjRQxWA6R7ZQIgJAaUi90OtmbqTVii%2BRNoIECMHO%2FlYCrCXivqYwZNk5gq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAffxJC5O0TlHfV3QCrcAwP2lJfNyBJ11G44Prpe0x6shbuy%2F2gn9wAcoEQIj8L55nMu26WcSqi9MeaxW328TdwgcQfNbEZLqUC1sDXrdDk0nHBYO7vs8kEQlNw2q8%2BWkYFriQ2OBJ5kInoh5Pmz5EOBGRTLtmXSEWNbdOcbCzz3occXODlgaekjZvSoDxRDFOHWP2A0gCw%2BlakDL62Vq5VgmgD9M5Z8Dqci7UIGBCGYYy1oknWSh8Av%2F8QXH2TLXjAqCXPQhw8548EAYt4Cxs1%2BqlfuKw5kyYHJxV71VM5GXYB1VEs3GatCPjQG4P63O%2BuTAgQhQNdq3gIiy8msPN8psSSOUzIZlnh%2Fh6WiBKTwNIuEMN35GOq%2FT4YXqSm4znSznYFnjrOJwpZR8bXfHavbPrOrOQVhziUqR0gEYiMvFXN5ijQeZnehIe4waHzfR1iXtfYGJt66YnOqarfifKDsMp9YAte9Dy2cnp8bnO85s6EdGmygtEvHep%2BkM7nnIAfFoVMogOD844xwZOJmAIbeS5nTqMgNNLB21t3RLhghpxoGEo3OQiwYXvxECYpOFe142WpNLq2Ql3rHT6McaOyMR6FFxb1B8m4WeGJ0Ppr3M6zFS7ZMmDxSthafzEy7ySOQbSoim6tWMZRLMNL4mMgGOqUBZZCn9YUhx%2B6k56ox750bRMIdQwP%2BpkENjEhUtW%2FCVB6ZVghXPzR%2FZLDNhPxeuaTDT%2FH3sORImhozHWQSRFcY7%2FCmMqTZ2KjLdsdSVmAcfs5eqTrDMLxIScXFR5cAZwxmeArwar9or2IPivt8uj8Xbo%2Fjmi0IqqoxzVivfQxXixIuXgY1Ap%2BhVlpJaeaeqehkA7Rfgy%2FSveZMRls%2FuJi48AuIpEBE&X-Amz-Signature=61ec8c0aafcd9fd3a29822367acf7c5f7740f5d575f49ef4b67a9898ecc40ae7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
