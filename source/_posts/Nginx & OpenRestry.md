---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VS6FYRIT%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIAJB4wJvtTsAeHyYxi7Pnh8fDohWhdgWhKRd%2F102Qm3vAiEAobn268fdtY%2F3v%2Btvn0u%2FubBa5k%2FkQU%2B5UVSpJ1Fq8scqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAU9%2F%2BoaHwGm9TLubircA5F0gFFQ0Em8zxgi0Vu%2BMCk6n0i5ufuHtjmKfbAY0%2Bu2dGN6XzQ8KFMLoxIxzz89HyzUbS2jsKeOa0Q3tbvMIIiFb8wrUdFIjp3alwE0803kU6LJGu1ckqe5Hql4CKLmVYht62pvbIxmV%2BKGpojW2L5krl1ciRuBR1Cywa%2BSgMyZhJxHjd39I6HrBSEuU6wh4w9ZOvlp7f%2FFXE9E%2FgMdcCh7wrk63sVbMQpMicYRII7opUrVfMEeSbHhE551opyuakfNNmu72KXTCR6c6RFyMPJk%2BWf9y3fS7QdVwfjMnVGT9beNSN1xmsbKa8pHCsyVMAPFMNZNM1gfSb8SnXCdqW0vpVtnHC6uEPZsIvcRPxXIVI1cSq9PZuNo5%2FpQiUTUOhJ3h1kDQSh4NjCdYaxrcsOkxmtMsPARX6qcb26gIIiaEXI5ij4Z8kLM86lbToq7H9moiPZxQ5fCZNGDe7rCvh4xryn4Af6jgjUBfs7WwBard35oIW1HkhrUhf0zZ%2Bm2Wva%2FLwi8nXS6skPmSHSoakSeXEXH42tzBIDy7VAqFyyE9XdKmodMEE6gPkP0g4tSbOj0vDp9fSXLwh3%2Fs19nvJVMzPTURvE7E2BgUbxP6ayXoxQDtLmmMCIgfQ%2F8MMue%2BMgGOqUBwWHuVEXKzyYHXwyZAV%2BKxWoHuFYTWBSPW%2FGwa1JFM5kFG7XA5a1GGCowcSHR%2FTKZRUEDVBgLSmE3gjig4JdY%2BDXxStA956rO%2BwT1RpKgm%2BGFcm7who8tWAGoRlp%2FaaFcg4si6kKMjHllzW2Ybh%2FeQ85dVPPTOEizTKd%2BO5jeGwHy39VN5G8Ng9lgihEKvqPW9406rDy9f6%2FgNbjNHTdzgKkhpXYf&X-Amz-Signature=068bcb03a1987e7140fdef4c06df6868faaa81e72958780c92df6917d83c5de0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
