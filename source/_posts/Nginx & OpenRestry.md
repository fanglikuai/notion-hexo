---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWUANZ4W%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFUnJW8Zzy5AMDYTCNLefc016%2B1zWctGzyHFWacsj0%2FAIhANq9V7YOZpIkvHEkX5%2BSsPcMA0URbpgLBd%2FYOLJvYKoYKv8DCC0QABoMNjM3NDIzMTgzODA1IgyesaIB6cFJdGOpQYwq3AOoj5dIeuEwzGb0FGPHcKsz%2BdGMTbJXYO7G0kYHL%2B%2Bqbbqlw%2BgYlxKL1buqrz3tSBAha1a3RVfOrldPmvN3wVUmPJMrSttChj7Tz546X5js2IUOpbYJNVaGhNiA88X2CvyTpsnn9S1tjlBtMlUgk6k0r9s24oZ6%2BxOYwfOe%2BahFOwTRSLIZUGQerjalVjxr3z8eLMryYaKKEHhkfxADKyDS4tNBhwl0Bp4GpXlNs6I5ogH8AtQwvnMztP3GO3m2uWYH9HTUxSWxRKzfVNqwm691ezVE4e%2FhN4ZFqELvZ2A2I25ox5HsPZg24c8b25T%2Bloz%2BSFgdPvEYeMQEVtQ57tKa2oa6wcrKlwCW%2BG1i68CacIUv9dNpLfo%2B4H%2Fa5pvJGiHOWuL7WOzIXOZOiBN3itUNNzM66SRt9ijxjXaJBFqt8Lw6dr5jKTOSpHrQUTW1YcitJhbmsV8YCiYy%2BluSTkTq1PTh1B2CN9CsdTD0iXdkKy%2B511CPMB8%2B5s5Ilevg33jnTDW3pV%2BDVuwDjkik54ANFdGcxhLe2pk5Z9iIdpirLzwNweQJdKAMCaFCroB1D8i2YxiTNshCljb6bgflP00oPi0cg18GxJ%2B1chEJzZGbQdK04kw9J7QL4WRiMjD1w%2FnGBjqkAVryLpi8mcV7%2FpG0swXeQ0935WyJGZ21cSOazmJruhTxu4oF3WNWBDOvcila6xx4RYnzp2VrqehfdvgpuHc92EQ6aLN7JRg4IA1r5G4MZ0vsXU46b2X1IGCddwaAlSTrUbdsAjgtRExDrQ%2FCaQdlv1NVXY3qbPK8H4eqLQ1hqRH8PQA3kL8qLfPBayAW0vUG0bEglz2szRo%2B3Hlx%2FVh38ZB35yxK&X-Amz-Signature=b8fe744c9d3ebf2d268f1ef4f51bd1abdca7edf87e64016b2ac5b8f55574eb2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
