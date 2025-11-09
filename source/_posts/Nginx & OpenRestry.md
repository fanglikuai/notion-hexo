---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVUPOMMA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T150103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIEd97A%2BluO240HGX6lO6aBCSprSCWKE6iS%2BFT7S%2F60HEAiAI2SNTHts6kcx8b%2Fj5T24jVQ%2FgWHPUA8IKLHhdQIz%2FxCqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlFX%2BQOamDiUSQmGbKtwD82mF%2FJoPwEU%2Fgvo7IAl4fEyGrT3kn4mNWD1%2BMlgPe1gPht%2FrR8%2BkySap931dZGSu8IglM9zkx2dTsZDqRy2uA9%2FgiUX749Birmd5ZAz%2BM9XjfVAva8dp2Dez52MoUPhHSmIAA5JPv8puOCCFSba6DqjuW%2BCcnVRLayYEnxUf1JXqeUCxdf2r%2BaP%2BrG1me5MSPOxgf7GeJzKtS%2FUkObCgef8iO5Z6xpjL1DT%2BF3PdJk4XFXDLsD%2FqEScD7E9yd1QDfB5SEm%2BTVmQbQccUxwsZgI2tHdrQhzBb0zP8jVsb6Z6tfgeln6OjhJ1VNtCxbTGTsqisYR5doLzq9Lgx9yIa08v45Vd1gDnAqbSNnpTjkHRjEFUfbyV6h4YB7X5YfAsgeaW2%2FX5RKdD2Tg7InxfTOLAgP55rJVC4BvE6u%2BHwC%2BK0%2BlqAePHmrN0w8ZUcjgr7AOReWctvTl3d6R23sGfTpDCUMySFPKc6pommRz4ehW7hhtu3DYIQNDQj01aIwfIn8ko0P7KyfrFclurxjgTz9tM2JL%2F5QCqtNcrkXtH59fhTuIiS3GmqbcRkgVNIWdJ3sJ25FrcQNVV93FP4V0DYzn85x1x8k4Ss7dnmIcZvLK3H5Dlj3lm0wJsc%2FBEwmK7CyAY6pgHdseNyaQ4ghKgAI0CaYgvLxpzRwE9Qq%2FQxwhtBaZDF8PpsTZc42iThZ7GG%2FpNjE6%2BkvsHvXYSBaiWzi8DCPsvaFh5bfFZNFrgZbPcBZBoUBOBHNIYMwMKH9yHKPFH4I5TD3VucAQzGaSN2mWJi7TeaSsR%2Bu67FScuY9Vlt7fc20XELJ8zGV%2Bb4tphZDK17mpnNohDt5Nw2ds%2BitcdjeCk4jbGH7hwm&X-Amz-Signature=6b963e78a8f3f7ef084bbb7da895b2113d5ec8d6981facee632e25a500a77711&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
