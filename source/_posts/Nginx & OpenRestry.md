---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZVABFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T230056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQCiFjuSe%2Bq5hXT7F9Ij3W33OIYGz09Wv2zzdwWH%2BCav9QIhAMlKP53VeEy83mh0iiTZsnNjKN1GmmFgjaMLlpTSeBHBKv8DCCAQABoMNjM3NDIzMTgzODA1IgzJAdfBODK77NvFgbkq3AMNSS4lAX8iuafQEtJVAPajUVzqdbuf7wWeXZzLtigPgUqTSTQO%2FGCJ6gIJeYvNNNFlw2B297iwVvoiggKKmxZlLD%2F3mjzSUaMdnkydlDQkOEDVJuzhrfo8sHh8ItRw04yKyg2d2pSYF0u6joXkqkd4%2BKxMqxx284QJzkp8vtc5GdtzA%2BMy7hxX1e6SILnThZQWE%2BKjRYNn2Z4i%2BIIHIf7ZSu5EvMtYYNEZwnovSpBmlEyzsFySfVgUq4st3i0IZ%2FFxEey10JfSzlqNEbzb4GRL3x%2BK1LM5WbKv%2Fjcyo4Eks4Ph9QEga%2BaSPHGd5k1FLKm7nMUGLEOTqj2CxPk%2FD1%2FW1X%2FxUwXZD5yrOnz4ybKmrPOHPEUsHCv9ryGToU9%2BIDDYZqXpB08dzPcGD9sAZbTTatjo0Y%2BqXsaEjcvQ91EWOCXgeeM1iIOJ%2FtTcwS2YxTCnfHOgdeQlHSkcIDgGC8OfSx3ib9%2B218s2eA1O7peCJQn1sSbO0Gb1qZaGYmucS1xMYHFhjUWon9%2FRjLqfeCcm1AsKMdwp1NsaTNb8aNxasEjceOezQnk%2F1zE3zOn2I7Wq3lZKtV6BxFXmNtjtpuSaq4KZye1J5X2ssLDbl2T2tLzkprtLBxUhftjdjDCWl%2BDHBjqkARi0Nh66MHwE642Gcxjsg%2FsI1wmWLo%2BwgPnTzDhakOmPtSa4n5KDfI3uR%2FRm3rhBNaYTuvOyAn9wh1Xo45f3R8pypQnnytv7CLpbKocS1iXP0KIDPg2MJuUG0qsl5wobbVKibjWOVMiPOoWWhGcm%2FG3vASf4EHx0kZAlpbIOy4B7ZG6gFWeISFFoIUgHYVc80tfNPX10%2FWIJLXZ6oqsR8rhzcJqT&X-Amz-Signature=1bb6b524bbe58140a73f539ec9d4f35487b5fa42dd79b50af2a8d7e2a2e06aab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
