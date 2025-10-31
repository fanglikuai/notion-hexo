---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEZQPJAG%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQCs3kcZFPgjUKbdxyxQ6mNDyLgMJ%2BUUfR2rDzHAv3Fu%2BQIhAKWDOh%2FbabqXGFMUZSIpmPrAxCvH0TeJm53YaADkF4cMKv8DCBgQABoMNjM3NDIzMTgzODA1Igw%2BYsOVv%2FA861XBptIq3ANZA3USfFcbWH4dBm60GDT1BlGQqSWE8O22Nwmp7Sdwx5nePHJLcv5Za7DWmA4EC1qTfaTcM79h9nrqvHWc3jGb%2F01Sj00tzdfgJ4wuG9J7AZH7aaudJwpwRaaVkFnN8iHBsfxsCteqb%2F%2BmVWBTodMvjyo%2FVwssv9%2BKoC1pS6is%2B9qgsjumXH2kQFB3fEDkH9H8qqV3NeoXfHCwCiZti%2BlBPcEgDNEQE8P0yZUDfLMZSFqVNdUp%2FyKudgVgRaWaItkpXJ9FFUWxU1aUcrR%2BmOpdjuMpF6W9N0%2BOf%2FpIExq7l9b2nwltQYCvZhf7sIPEFd29LFMm%2FXfuHWkcp1cDE5ra0hvSi9FkFJ00l0jRl5HpJT20BFK9tEnhwwdysO%2FJ8xfYJlwfaWWYRuGyUPlchFiIxH7w8uV200ufno4e5lsH2BRong8A4h0LYVG4mzhRRyf4EWxzjIq3nyevhQ5RK8ET6jwXj1HsXcWw8i1H2iPVjuSRWyR1rhIzFLZhaj0gHjVXpUAM19t7e%2Fw19gn6Qma6BcWAsYRMO5yBRTLRlZt3h9DbNkFHMu3BaxbSR2foopfa0xJdAiR3SGLIu8VfpiFsJ%2FOaKQ3JX9RFkENsh51KvQ55a3yJd7vTiXgOYzCsmJPIBjqkATiX5fxtTiS8q3fs4meFRaW64nE0Fo9xabKY%2FK3ZbhFxX%2FLDb9xrwJx1yy3AMMtxGMfhn6bJQQ%2Fmos8YQbrYuNoFDGB%2BS998NxCeeQZZLy2UfINYtqWrD5sc7vH16odPgkWYovtTJAvQeQtlmI%2FjxUsoPDLhO3RpTlp5ksC%2Fxpbeh7mSrtRVZteZVVDyUJRX6naZJTchwUm0GWcuYSv%2BhXF2tSji&X-Amz-Signature=20a2919ec0194f9285465238a34908d040a78a3f9a988910c23630da3e0c6c73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
