---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675OY76NH%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFALxfYn59Q4%2FiZxlKVhrUpT89WjdR98Rw5xnsCFD3QAIhAIDyf13O%2Fd60CBG%2Fnkor95dNnkrzlyWC6Xc%2FlBd7QSKDKv8DCFcQABoMNjM3NDIzMTgzODA1Igw5YSsT71wgwo%2BfP3Qq3AMG40q5e5sH5h8neQ9CHP00Bcu8zS8b5VZ6ieIogOF17FcynxWiG6GPUIAlniEbRbk1yI54T7oP%2BiY8GFoJ%2FSU7XbFOA1q9kwO5XuHz2bu5%2BxwfN2kanxzI91CCfP55g1UqclpSN0NWNX10jwgyfkOxGxZfRPDuVrVqK0dIb1bKi%2BrIm49gsw11imS8o0OdptdtfatKeduyboOUDn%2FDi66xmJ6H2XLuVhwsQkwSFaBkvh578gO3WeUbC2d78rASVow8xd8hwVMire9YpZMt%2FBzH2C4UZ6WOMfwM%2BF0Aq2IlgV3Ou7irnJBLE2C5z3e%2FyTXxAr6VrBw27QkSLsbo0aUr9GNbIt%2BnvxrNASetTUNMeJWwenHyPRLQg29IYLfAeds%2BGr%2Bt19eiGIFZDeVXC7ItwWF7DtssME0PYOtn6bOinPApkiW7k95oFU19PiSmzMkMfU%2FOzVkwPNB8%2BCYx0BrfShQRl%2BdP6GAlXN1%2FnevU0pJ6VqYbci66yi9KM9Tnw4yMGuJdyC5jwiLBfaECj7RP17Ym4lL%2FYEn1ihM78xUX79Yknj5YLLt%2BFAezixdJY3TaGyssKh62WArLrf0GG%2BpOEHNJlC8ybohQJ5ES4OQyoFrasBbNwhA%2B7e%2BQEjCYjKHIBjqkAQrulMchr%2FTsB%2BQdGez39oen5lRZYUeQRNcruJaFG6E71IP5YsxeKsz4si8pcjT66Q5Mu3tyYxengiRiYSsmvCJphVVJ3S0W02bfmzOXDnpGR4OgdzpE3qcMOGzMqQavGx0pfnbrSHlPJq5BWLe30HCXaohf4uPqnRcaT%2FyB%2F2w1M5o6FV8sXFHuu3mFIFPLWWDvcwDh9Hw6W6qa59NvOD17jDZp&X-Amz-Signature=1b190fff4d52f29b9d24f9b07a453ecc999a1e94eeb36e7fb3ff3301b90da78f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
