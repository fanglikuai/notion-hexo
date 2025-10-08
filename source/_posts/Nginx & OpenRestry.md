---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYQZT5LK%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJIMEYCIQC8Qh1DzrCLf2V%2BZJI1LDCjFwdfTqGsjkZaRXZIJ6CU9wIhAI%2BQY4gTFJgzJ7MyrHOUhbH2voU4ft5eqRs8r6fCq7QPKogECLH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz3lB8KEYhg0L7LqHsq3APORg%2BKN7Dt7GEC87BmQoISVmjMEFBhHz3KgkXiBYt8WRZNQNpJNGewmUsEPDkzB%2FQuQ6nPIpVE8wdLuGvmdg085J7ns4lmTCzO1zkA23HWWR2qxXEva2Kfyqpt5Iz6FUmlA0ScvHXB%2FnGeK%2BqUe4uKc1QKS4sPnFZ4mLeQ%2FCCbMPINHciu6P24mMobfyoY1tzagvc%2FZeMdohRn42b7jkc9VyimDorPj8GYDUoB4BV7pPyttfE0QTU%2Fd6nMY%2FEvAi2hVlHmrRsn0d49JVNfQh2tGk59SeiAxxxotFKLnF8hcNhsA7NV8QynI%2BIUnZfbZn2EBo9%2BmYfwnca5lMiB%2FKUj%2BWImnMR2Xpo8ddcmq56gaKKGLeG0bOfmDVNm3bhIOptMIiHeMfhfZ9x6m3N2jtCF20BvgyrKCy7s%2BhQb2pn5UA8jSQtgj0QCvCcDKXYR7AqrocI68jZOgcS5llhgwQ499pnahqtV9l71KvFF9s84a%2Bijf2SwuTwgV3FJBWsGGKOJAQS2gFPLzy9O9nDSAtVQ7nFRrxlpas30CI%2BsDoxQdknWw2QvcLmiAiVGvX4AwR75EdzO7yUAO%2FsET9dLPcBMu1U%2BwhAgTgc5Ge7eAiF%2FgP2pBV%2B714t6Cg5IjTDu0JbHBjqkASuNMr82j%2F9%2BjjabbLw7NE8e8KrckBRmrpL8tpTizhtrZFbomI2QZQdaDCIxodai6bHvJhINVCP05htVY1pvFSqT%2FNgndd102UlMpqxCGlbpeq1MHiNpd8WR%2FU4jv2kmJ07bCuYlt9rrbiuS%2Fvt7kbgvSNWbbGHXoeG4mEjxpPsIIU5AH9%2Fs0ft3T911r67DjHrtp82fNeDDJpuVRc6s5nnfbRKf&X-Amz-Signature=25f2e99922e4dbab3cf354403dedf8fc24acfa4dd30a173ba9295d5bd150962e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
