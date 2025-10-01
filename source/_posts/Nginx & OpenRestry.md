---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3AXHV6B%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T130051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIQC6%2BmatTIsI9dGHgpDinQyHL5RZOGRXWTlXQkWV0%2FiYagIgVWhb4PvPmHbKjtvi4u1G5c6QhKjC%2BiZCiAO1OCTvNLsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE%2FtMSvgBlFT9m4nISrcAzWK9H7LL6dejWLXed1qyecp7RTtS1dzYq4KxixTsd%2Fso39WglSP5Pd7nHiyO1eKt4qIjW%2F3rev%2BbvS5BMo0%2FgQN7kRtHUe9hahXTxlAWWuF%2BcVgl5CNnqFMZ%2FxituBs2ZCKLHtkLPzR1O1lA0ibT8riEyMEQ%2FAs5YjuZxS2YNN533WebcvRkqR3jTzrttz3Kt%2F9QTAikgH74ie2GvnqYyLW%2B4hDC7VBh5d9rDnj1aCFMvht9rfTCYeDj3Wkj6NOawG%2FxeLgT0rO2drh%2BXgLli90jkR8r%2BUL6e%2BQV%2FVHJrtz%2FWrF5X68YjodQB1HoEx%2FHWUxBVu0yllfSxQoT9CnWU9TgugYCfCtQPwjC6lX6lk04%2FmZXyVPK4qQP9cSEPotvn8HTzjhBtH13A%2B5AydDtHkcwyDEI1k1AbEGmsvwnpZQAReRVcXV1xLsStsFH4JITIrPFQh7dV%2FboppJVI2vQynYK3wbLXfQxZ1DqRgfphRWV0%2Byldb5aOSCgOz%2FKTCBmPbVWaBqn3yL3SndVeCPjhLYdYSygKUS7gKaIxR%2FZoE3FjJvx0sdOtZ9a5ESYxpuLQp4WNTH8ZstRC%2Fn7GktNzT4Un%2BkS7GPeIvMNsJMmMgpWe%2B37kS2%2F462dWn8MJq99MYGOqUBkw6dGgZxWsBqCNj2ECBjL%2B45WrvWSV5alZZ2lcppUaVXN5Stp18V4OLhpwmRHDsyOACC8hcCJyNGg0N0P9tAy8zHoVPCaoyC9PIP4CamF4pO%2FLCQD419i1ZZXqZNZkBFs%2BXI27oXgvb4aY%2BYPWSI0qGU9lFkrU1on%2BnCI14jwkIifKpXaiksL7O1AlwU%2FHWeY33Ij1%2FpW7mfFKKRw%2FIMjsLad8hz&X-Amz-Signature=062cfdfd2a9f1b11957b3b42bd145424795ed9f5ea90779146b637decbd246d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
