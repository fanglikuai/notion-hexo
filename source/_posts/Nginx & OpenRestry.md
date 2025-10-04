---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665UL4OVX6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCNRw6x8bAdVGOkSVak%2F8LGjKzlYvf2sHZGCtsuFXNwaAIhAIBQd1nGQuCW0KuGhLQq4Pr9nF1AwwyiOsshc5tOPh8PKv8DCGEQABoMNjM3NDIzMTgzODA1IgygGmeKWubAdmQxaqoq3ANQrsbnbwPjbdFIqLAc3EKVasVu4tRXgdpDJ0cLoKMDypFzd94osT1VZASE5rAzzDFKL3tYBwQVspmTQb5XJo7MJv62bKEkdHu4gtIbqnvEJPU42lijSi4l9kIGdBgCgv4DFADqEibQFgqveUa%2FnpooBypAscxZ0UEqZk0mpv6z1zF%2FKjsLS35YpttzyyeGFiOnsYSRQmj8VihVdr7I9%2BZGbLU15dxNp%2FR7kiYPQSjZ1XnX9LlLwr6agHSxIhAboMB5FvlCEl8Iw%2BuPVLOuoHh1KIn3M2W%2F4Kz%2Bsb%2BvEGpwyQc7C1X8LDwJaIcdQtGd5i4e38OK8zaYBWQbRozbr%2BfSjdzHACybV4YvxVa%2F%2FDo26l4xMSoFyKHu8ct7MLoZFx%2BJtbi8EdlAQEMuK2O3X8Ekms3NpnCJ1M%2BaQa4Hu7TLI5%2Fqw6VXc4hNa0glIQqpbqBpvH5GCObM40ofK2j5N%2F8H3g1qWfTAB%2Fjuk7R1qOvCVskF6EPDPhJDSaFq6midikimaiKh90BdFG9BGkZKDZ9UtSfhk3xKHRfC%2FDXT6DUxaP5MR0pp3NgqqXe0yM5SpU85OQ%2FTVeEjQBrWn2wxx7sjqQZ2ukFlcfQMg7MaLOTEU6rYRLInTnOoE3lqrzCTkIXHBjqkAavIK4FnUvLyYa4DftLGJJFf4LG%2BQUI%2Bm%2FtSYWCkpnA0ikgoBYKrYCvbL%2BiVtiv4dlpZgSiYUPUMq0U9Qvjuw6lZDg9d4nqtaea2EcmnN6iXvCjHD4CwMkf0%2Bo2Y7UmqbGigRjIFWlIfna43H4OT%2FR4%2B0vCpEfbbVphg4MygIHTHB0qbNV2JHRZodGeTb5HMpuDOqhN0nSsBhI0GFg342n5Zuifh&X-Amz-Signature=191a06c7ac85b1928a57192dfc706b4a453dae7cb1710dcc1479815854355811&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
