---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMN2CBK%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8TSlDbJtpa58omMHsbMPKoUQlEVk0qO7Z9VZ8Qxid%2FgIhAMRf94t8XeUifryoCnXFEQC4XSH7k4eHoulFh7A1m9CZKv8DCE4QABoMNjM3NDIzMTgzODA1IgxWhEdYUW2wbPDy1Poq3ANodHqO0qhlxlx69RwHiX1K01Cu1fORnnrzNIVz43exc%2FOPB65Fu1zHE864cIC%2FPwP913e%2FT9BbelxBz%2FO1xIJdxk19ckZM8G%2FktgMSf11c811O3nVvs65iNq5yGGuGxCPuLWnNngJcSjQHIGTc5Eb9E%2BKFVkycXnPxtoDQnn2CZSHLbMqJHs8BagJIjqQuZ4K9ErDWR%2BlsThYwuG2sXb66t0h7cenboQOFGqDpiLFuDxsYRVaqJ57gTwUKy0Xlya7Z%2BcNGbk6HnX7kBVmfFMEMAO8fQ7xXemcwqRxx%2FlDJeym4ScW6mTZU0O6XzMJ455IAX%2Buq%2F5Askukqr1DYpmYvLqgQDvODPTJzip6x3vRCeOxWc8WAe2ovUvWaiHSySCLzEEvPGwjRo4KGRxZDokcxBrndTptFzVR%2FCbCUVQMx1fCnyjDbmLJA05d5QuxxgU1zxzPXty0h6RXUnn5yjiKWNckkyt6APRPFcAj6NH5%2BloOnupeXkqaqhf4GcMlCyrIwGO6tSUTQBN%2BWC4KVFe0y9XXk7FU%2FVsvp2%2Fn4qnOZMq5rD1WGL87QZ1eJUOSuDrrK8%2B4prTUtI8umr0%2BZJGa2U8Rj%2Bm%2F2eY7SmZIqgWgaoP0bhxUy5eRgO%2Bw4fDCV%2Fp7IBjqkAet%2BmExmKQlnsrDVaHPHzuJvCrGHQlntNNFKk3GoSLVxV4fdDyN0tDD9kmV2oyr141%2BzKd9HoU0I2n%2F5vxBgrBdp27V1Yu016FMlZG6GOfPWBj9gIy6gLknRP4UjlPjn%2FQ57QnXwz7pi9syVtBcPRYb67kJje34WsASayVRRaJGWhvRWsnMtuMRFK3948Ee2Ljr3OiU6PAT2Lo3T4Zo3uf%2BNz9Q9&X-Amz-Signature=4270645436bdedf7569cf4f19cdee9e4e4d80661b9f3e6e9950dcd38cf2e89f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
