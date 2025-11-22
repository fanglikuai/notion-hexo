---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VIUVDUI6%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQC%2Fal4tkOaOzUllmjPH1mG9ASzb5Upow7lL4gaD7xOwDAIgLNrgnLD72Fs7FJzDzbvoNhaAz0Tv%2BqcFJFaVsqgkbL4q%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDN5GyRqMAu0fhxX3AircAzsSl4bsxrfbXyuGBdhqrHVIwQb%2Bn%2BkF8mQGP%2FK%2Fd4Ne5TQ97hM3bJNFyh0GqmtH57ObTq4fPvogJ6JEgY8DXkTkltV3%2BDScHNJoJRkgGoUSMTGGDc%2F1WUH09fZnnh7YkwWQ0Qj199L8F1669zUnwWd3YhDqknDLLy8tjKg3fjevw%2BXw7N%2FflsGmiz7bt1dgV99UtCA29GfNRdVosPYsCbcDFzvDAiZFHbTH33rwne%2FqhqpVymZ%2FjPToPReps%2FTDp%2B1JP6Qfngc97Dlz33ypY%2B4dzbDaicjQbcsDlJNrAFhGbk6vQHdd6TrDyLStR%2F4ehCXJVvLGCVVUGF%2FAQVwcKB75HITn%2FfmaVPf%2Fd6mrZ%2FfAoqSzAx%2Bz%2BSVLLPJx0GXgWpAiGsU%2FZW%2Bni1V6H5ezTieS5yhucSkY%2F5BRP4MsMbl4qdth%2F6bGDnQ0SboHeLMsUtvGG9L4rtMNklHDMBZQJqnOnAdv0vjbfiCx%2FyQg%2FJzUNGOepqYYbVqv1SKVgyB4FrObIE4PRUNAEUrxmPukP17APn5mytozdBGWqE9MyJzwWhcRBwptTWBy3RZvX6GvgvMXiAVYtEP0EU8nO9mM0ihnjhvqkrwnQCBWlQFspnus20H%2FmTf4Cwo0eLipML%2FFiMkGOqUBXAmr3CZN3oHn6%2B8EmMnHbpt4BXlbQt%2BpkC9bV5f3IcEQMMScgBBCSIltdtlYPd26NNmxN49gi5TCLmfUUoqvpmtHqLVq9Ss9M6dhZKVZwouAxyBRoEGaDkE3r6PIkUBUjje2AxyT46%2BdsfBOdSBEE1Rt05TOGFbmkQx%2B0XqXsh16px1hDK46BWTss6B%2FYred05S2hGp0NzbAy0Cup0Qx773CjSEU&X-Amz-Signature=8263b6206a9f747d646159a6ab27a25c0e08d08ca34215fa781941fdd0dbf80d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
