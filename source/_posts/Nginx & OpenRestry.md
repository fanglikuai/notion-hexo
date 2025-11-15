---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTNZZ6AZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCEcKgiwzHVGtzAnaSBm17WZbzrtqfbox6wlaJMVIeohgIhAO8xxsDGtle26kfjnOqQdIErnOirQjsE2hsYTbqurUyiKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwp563PYWNwmfzry1Eq3AOi%2FeF4XUS6O%2BYeXxgxRp5L62GFQO9gWTQS76mR0cUExuQQJAkFH9HZBe5zNhbuSPwWPjOo0FaDnVOD73CfE3F4fNsbzXpfNjLmCJrLlSY0d5AgOtSYd1OJRydc9PGow4W9%2BHB8uehtn1qvf1izzZsVIXe6%2BWtpSdntj4%2FE7wt%2BeGDIhGpmFa0wvBJo%2FjQj1hvVz0hsPkHGVMMrld8GeicBZjQOPmajc5EkWbW06DpL0p%2FHq8bCnbirnEqTBirBZrAkYtLakqVzKTiaF8l7njgei0aM9tbK6KTRm8v2rpff8%2Beq3oorRqZemrHrBNblJ1Mz05OjTnAw0qozIqD1aB3pw5mYltv76MomMXlBNlR7DvGlJNhXIAGxBgFk1B98gLXWq2uOXviDuOSmcCSJd5%2FqOS8GdL1l9lSvNETCWHDCcghdkqLTL%2F7JB0LP5ZO0Gt746RXP1FkF%2BqtZhZd%2Bh8k62EbhsE3di%2FwgQ7MbB2Boyc8riMsq8fXF3bnWq%2BierjLq1yukJ955F0S127QnYJ%2FTet5CXffJ8UFWaLkECqFOqCFn196t9L%2Bwh%2BKtbyZ9WE7noPW5FbYaddtpVNvHfJCFqawe9GmAe65gs3%2BM6zSf4J5MXAdOqOU0HJ5WojD9xOPIBjqkAZ4YQBiBVLvhvmhOOUZI2QAZGF0NK53QuVS%2Bw53rHYVd%2BhdgITw1KHNev69OUGcvtbS6o%2BwWydeiHhOyX2TvF3kkzXsMSFobl0x1yvBBPZe17cpZXEljrfUnuc4NUorHnzx8x30Lm2nHA4T0hTiZNCgn0h36fytQXzGBNab%2BCrxwE4rhqMZEi9ZR%2BcCC7V1I%2Fq6chQEMUZ%2B7kbPkiG%2FKCSK9bOwU&X-Amz-Signature=bf3aac5d4674300c7a010a617efbb915869eb8508013a858f05662e4ec7c9276&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
