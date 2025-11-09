---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAXZZ7BR%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIAQk5e901S%2FLMTdK20kJyRCtjivt9qKopHtPl7NT9wU1AiEA3x1T5UUaz1bbRVcG%2BXCNPOmKQZwIErqfq9Oc9Tay7v4qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGIxcbM9fF1Qbp3rpircA3CiZtquB0oCJDKPY74GMg1BpfvJBIgTRdC%2F0kj3ySEAyojzKIOOkJFzncc4JN%2FpvqLTaVQNJKBjQ0CPtpkkWDogxj6AunzVqHF0zkJyCk3%2ByMrTFreTR64Nd0LiU5Ln0V7H2bDCd9bpwXE9HCUxIYBgfo%2B1%2BxyuQAUuZ5vq%2FLtuPc6pnIrE9zOvdu%2FD2TPbZT%2Fa%2Bpz5VKdXKKfWWiD82L1K2axwfEwkGXP8HlKicL22od%2BBZxJST4R5Bwc1Lhcvo31%2F7ZU0Tvc410XorNiZthLyUBdpDx5%2FF4%2FhR8N3ooXoKbUnMWVcEzu%2BB8Ij0uDGXK99Z4vWd3KZ6iiz3wZJqfQxTxcJ8hSO6K%2FXIetBD%2BI%2BtAZI4XQFD8U5NSQtZC9OYr0d1N%2FeLCxGic%2FWVcIuabxev8qRPaPtbEnljagHrrz2CH%2FdDqmAB2lU6q%2B7E%2F1XQUam7KKS9ypbLGejGR5vdbAc0wFkUPUfnBMjgErk1DaPkSP34flHFAh1lBO2PLVcUxq5TNcn2eXSMhXnrYzz5kN3nRuAa9GjQp5lacYapqeaN6S3h%2FNHWfo3%2Fe4NZwMzAI1kv%2B9p9mhMpACWBl9DtyxOX1mvBulFS76rzuJ0MDyHnQrLEHk3Pq%2Bn1KvTMOaAw8gGOqUB4FDLbg2zyEHBeKaDyIJ%2F9jT6RB5UbvLVLkXP%2B2aObaCXZACg5Kzqk%2FfbjVh0NID9el0pcjAN6qfHElx2jwHOa%2BRyvqBsPpms3XxpIcmJtv9d9bcFCtkGI875zoQAK%2BClIEPxE2BmXkCRPvmt4jYj2yqLKSrBsvs7c4v3iOZTJ7CUYgsMJBlWUrqWvE4%2B3HWWRqUmQhUO27vGYUUoJcKFANLMbPOm&X-Amz-Signature=897c1c3ea804d4bfec1a67a74ca6f38ecb3f33aa738706ef663a8aa7204105cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
