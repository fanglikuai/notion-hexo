---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF7CMSC5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIEXGsgrS7U3j53QIHY0FXHR6oUTYBedrbZxTAnWat9MPAiEAjPfDzCARxtX2WFVL3IfR6wVCvacPLRyHmyKO2BYrGg0qiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5BT8tIHjXFQRiXYyrcA6VPgBeOab2rGPQ9KsqqVH4ilmYNNJ8ZOWNmLpWEdAeQr2MDZlPdoA5zaWpTOHjrBJmC5ZaCHdHCdAqM%2Fl134AAqAQeOMFuaeNiu8hXJ1IXh9IbQa6eG22guzfNZnDwnodyT0Qcd9QqNhTZIqBfrcIEDxZyRvo7aCsdLN2ILOFwsf0oo3SydYNsFqBZ5xDxVF3vDA49%2BnXd1y7P71VeDw5X95PAUgQJjlVDgmyWFfwMiXc38D08A0RPGTIRExV0wyKaS9dcg682lh25EgLqAIbsFIWPV3lSuYRHs%2FXRRLepKRNXBEuj3ISd02W9Dsc68%2FS60bzdBqhYSX4tcHbxHESw9dP5oMjp%2BZwTIpJJJYowW21Q9C%2BtM%2F3SUr2SLp7e%2Fip%2FLiJaeTjkH%2B6BoI%2F7zHJPegQYcgeIvwwLPrf7nMh2S6GByDz4zusNcva8sCn3e%2BSiVamzHMQsBgMBElwGeAGzGThnu3g6019voC3WtqYGNIm2kOXC6rZ%2Byip8qQNGcd%2FiIcN1%2BkMCLF%2BS9JmBgGhfhENBxn3AUJrwFSLZJYksAlwoFBnIOnhLJRbl%2BMYBhCA%2BouAj1onZSbRvd8on93u3Sus71%2F63VVdKsiwoGc0IbpyofmikBfMeEUDQRMPPTiMgGOqUBj8ca4kbdElawkmO%2Ff0eyCSRiL5eawlAJvejUxEWW6nwREiYgWSu4fazCmxbFWBPxRGUKT9yyPuXdThuauRTGS4JM3zH6mTsBANj%2B2A7RqNV2zCDb5r1Q2mvC66eZPR%2B60hFJbJioIbgaglgqaYSvp6Uf6l3EKPkmDnwOVy0la7QdkcAOIPoSn%2F%2F%2BlY7MonU9lie8nzmFNLjeIBsXwKWqGmA0snbL&X-Amz-Signature=9de924a55683242c92c339aa19da4a4840264752470470c576d0e4833de52785&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
