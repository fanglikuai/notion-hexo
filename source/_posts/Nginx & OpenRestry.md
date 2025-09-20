---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674JZJJHG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQDz1jFbk2HPxZ3TnmrTdsm08Z4DeNVinIYuyVIuVTXWcwIhAJNzfaaxZoEhsHw3ddA4pZzONnspjDAZedhwQWaO8yniKogECO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxxCLPPzYWiR%2BHnx%2BEq3ANSzxtXNR9deGFPvMIuUy8JIPiQXT3y7NHal%2FpBtewDLYIykRA1F8lQJptUNCf7GprygeWv7R2o185s2vNhwzLuLP6igjEFouLvLub%2Bjdu3jYlIvf9n3Uwa1Qdui74dm4YUmJf%2BoHK7UiEXP8Tpw00YcitA%2F5Xt4v6pBTspgvfUIXoM5miV%2F72EM%2B23p3lZjqfK3NhNqaMlAtnQRb%2FLaeABxLFB6EXxq3LF6EYfjGEbkQKSrumx84erhjnQGkO1vIdryCqCrWYTdzqRvJahZdDNJ8E7MjUlM1E0wni39lv1axWaF9KxkyEjsVAroq2B5RdFTGilg0elgp4ZsewTU6uNgwqYx78wluMWJIb2Cgu%2FdYSitfohourE8bUyXZEtvfAyVA7uRhJW%2BVi8gt%2BgEcoLJ7paVt%2BFtZUy%2FyYt5RzWHwfn%2BVrWMAoMkOY2NBK%2FpZNrnEli%2FNEHQ%2Bgi9Esm5l%2FQSsm6QAaGRtwOnOpB%2FzcfmMcXOuN18AItCUbwYpTtzGV5xLcDkCy9GMV3X0ql6JO8IoWdyVM6z26rhYLRqBOvEKax3tFGPDBuji1QYf4zQUKGi6HcMyWJYA3CA7zru%2BslNsibzvF9L%2FJNSp1Hii0z0u38ie5z3akk9b2RnjDOy7rGBjqkAZ9FFm05hDmcqrTLbcHkAoJ6OtuOm5mb9y6JN2FKSnHDSzSXQbJTPK%2BgLRnGtUYndF%2BYGXI%2FfPsVOVJs1%2F%2BupkpvR4azRHSJyF9MSFamDjnMlXfnNfhsw66ZqBU%2FKSZxYuUjTHPlI8quPbxTI3tsS5SecSAj28lN828zuHkDatLW3rdb5R%2F%2BTM9QS2HbB6rKPkaW15kKUYgYDZMWIjRFN2u%2Ft2D2&X-Amz-Signature=4a7da788e5f8e6b4f93e86de8e2754afbfa5f11e103e78d11688ae085661a691&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
