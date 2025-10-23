---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZATGA6UY%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFR3h0HgHbvG%2BSscKLmxzuHF3zJ6WKQ7Ss4cDmwQyCkuAiEAl6gTRkNenVFjtNQ2kEJ2unbfYXD%2FbgMVDZ2YgnPDnAMq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDOgy6z0OlS8ycrE4%2FyrcA30jZUJG6E%2Fz7sWWu0gneEkXbTGnCzrhlAwfKVXd9wsjUE0HnuMtKdDPSf14WSwT2jWFLz5nbWmLWL%2Fhb8PNo1cXj21ZkKBhhnSyzDitbD2MmHd5%2BKR0LFgmFg%2BvxLrPn7V0OkB%2BFLpKyIwQ0%2FyleZY%2FWOVv%2Bgf81EVVc3XzokhTuKoEW2E1TKpNOO3fA%2BX4nv7HA6X%2FCXLJRRlgEy8o4IF3RaWLiYnG8GkZWd1v0Sck38pYeGSN0zD8TfpfC7S8KXq2H8uIrf4GbIm6emN150rqe8EwalhUFmxbC4FpMO%2B4rmQq7ONKziW4FjXImNdpp5zFf0049uVfn3sNa6yymzqDj4WTGqj2VRmwNBefCCSj5dhiYOn3hDdtOVswvhSyC0bFO4FuBygWYzfjaUPeMqDMVZzRZo%2F1pYF66Zz%2FG%2BNHLMseRRnFslL6Ku5c%2F1c3yDHUnJhgIt1RtfM3tCKG%2BizKC5h%2F68v7HeKOJ8wkMb9EZkjeuqgOZwTEffUNZ69RB0g2osB5a7%2BYIPwICYfb1KfQgz%2B9RFRTO5CYof0LHoy40zJ5f5M5%2BbH6Lr6TPSFUtW1QYHae1QrjVaAi3L%2BWyKhzy4ryFWveyeROTW8epZu%2F1VSI5GMlqZfLd6QLMOje5scGOqUBa46g3AWD4V4FYJTBxwtm03jr9SygBKxXUs5K%2BJzZYYPMuY6O9D6mTjehACIwH3xu%2BmcqCzpFDVpEfheUGioMrlA2X6jdbfUHtfy2OuM66xnDD52HPB8Fgm3PcsixqjiC%2FCARhHhuSImtoJd5aG4ufdO7anetMgrSAdFF%2F5S3%2FAaaymktBOPlwqL2HsoBLhNn6o2MuT8p50zBMl3yEUjubLaPq9mR&X-Amz-Signature=5b8ee00cab0e7a46dafd00705dae7683d1dca2e3b871145557c22fd2c97cb945&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
