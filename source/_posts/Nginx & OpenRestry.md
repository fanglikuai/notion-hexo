---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KICVL7F%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiQKmZYWDL4%2Bx055TPq%2F9ogxs9UJ9ZKbxnkesuHqAoNAIhAKBGHaLJ5e2npbIZm8votpiFa3w1a8piy3tRbfrYUay6KogECIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FfOHY5PSv22jNwO0q3APjFVDQevvNbs8TGKcTYk2JsOStIwJpr5vKFFTFGTo2rDv0wb%2BNuFKJCrtbGa4BL1flrz1al0DGj3fi3mcIWJxAp1YJG0KVP7GbSU7fYGfh343vNt8qh6kmOcUXRXsDR%2FaO4qvVdjzigAQirBylLM2zNJ7pcs5qPaTi7PPNys5rZ5wLVpyEqur9i%2FuBC7vFDRKM%2FJOKwd5Aiwf8YjxjAg5Kr24jbUPMWyt7Z5nn%2FFgjnSGnpS13HKxYJlwuD%2BWIEN%2BHaNsRNtSscdA4BRa8Ygk2Bg8Tlpq42hmRrP6U3IHq6qA39%2F3uCiifDpA6lU%2Fn%2FYWTtjJubl7fAywSBARUourUIrADzYd4mCCR8aWZ7Jdd%2FptkIhzpcOxGsyMx3fau%2BqNGUo%2FbT8FnIMPHeKrbA52bbWbA5D4U%2Fj8%2BbK6PUH9eKTLUHOqjhx92sffRstPSyZmBrQ8awmrc5qV43RvEMSAKYX34neQJ9e%2BDBS7im1YTs7XpW6KvJ%2FHJPAjE7HFiRRW03dKefnAdPXyfJqUKnv%2BUTBiSMt6%2FplrlPz%2F5Yniqm5PXHPtjgv3QbxUfyGJ92u2nFZ6Xwa638ApkA2avTq6smAMwIRAAP2tkmxzkAz5%2BcNZvO2s0z4shx9oJ6jCR%2BqrIBjqkAUzmo%2FFrT77Ax%2BM9pOSyJKInYbgAvnwbg8Vyg7lfFlY8%2BEGsrRNylBql6p5fJjvZFWsaJpI2%2FxJaMhkzQxq7Iv9203tyB9uB9cAunHD6qU0JjeijGbMdhzLzdNDzybOMxu8u8SOD%2B88zl8oeT6RO245znvgBapNMia74oOi8MpQgg8kbeIlz1uSGjnZ4MmjpLPKb4xDfn5qzSqwpqbYT3k8Wb3LK&X-Amz-Signature=735a6194c8a9a179c4b5a04effd6d29ff53ea1c15f845f57c60a81a3581842d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
