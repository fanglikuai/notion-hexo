---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TPMUGB5Q%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICtdJfzrT2y061n2jJ%2F1Gop%2FPzNdejZxn0BiezFipig5AiBSWdgqFjJmvV5RSxxyejZxVhvcLai1Y1yWsYU%2F2cbpEyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhNd6B8uVcgECethLKtwDxvLjbRNR6eYgrFl6ktiMqZpR3GzY1KrgWnblEBcqewrC4ujWeNadx53ppwoOgqa0CNWPaxd6PFQUYP0C4QjJaLNzxrrqHUMarF0eeQrstDMqtZWDKUMJrSRxHLlcUgTCfIpLkqjQ9USHN9IgqioRkr5OTbnBO3io8JX4Pt8WUOnuVbzgeWSfSqnZS4rW18mu%2FgddLM4IH3rLJ6WpqeyYNTq0qnforfojHDtcQ%2FDlhzoQ%2FOYo77UQrbEyvQ6Uszkq%2BJMKYhL66qWHdO7mr%2F24Pn9%2Bbr7d6nQvQCeWQLKguqq9MT48AMm4DWUfjVj9TWvf0ohEbD57OH38qqrAx3KGhOjQ15JZR8kl0JfM%2Fp4gN5zPqbPqhDXr6yx1UiVfnwXaiOtmf%2FYR6XgMNdFhvpBXj2PsSSzAx4WoiLGiK24pXZdIlzl3%2Fi1zeInXsOXBjV1Q59oWY2N8o%2FhKqYAdHC8p%2BWy9E3WYDHMOWPtZsSDe4gMxgELs5Wv7Oha2Xx9YXmwkA7dBoLn0wRD7vf1Byx8KoeYDaykrnHsbDGY%2FTI79X2uedOItryyUNTYZ7h7FrhVzPFP3uIvlIJEHw1tVhgrl8UXmbISMxtN7EFES3AixK7i3h2Qm4rnq%2FfrFdjEw%2BqOyyAY6pgEdHKPk80cq%2BdGkTXJcW5u46SA0gRv4lAJcY8VLD8ECh7ylWfZgw1Ip22Zrh3jkMciOMBY44CwTn4kbkbKjZTICr6tZW04j%2FKtP2Y1VLz6nnp4TJOJ2FAe4EB89yAuN4vkPBpqcTX77jKEYT5sAj9Ia8SLnviw%2BmnTkOdLBo%2BcBCSclbTFT6YaY1fpjd3g2SQQdcIIPx8Zt3%2FDYRSkcrJdKL6VWro6q&X-Amz-Signature=7e3b8748eb16a440c100cc73e2ff084fff58a80f18d33d39c8e602a227bbd996&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
