---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNPCC74A%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIFpqWx1QMX%2FKxfPE8msVzzgfREExEQZ8QLE7Q8hM8DR8AiEA8FuAHjO7u1P0UML%2FgRWthNZ63lPm2n7W3xvC7LgTeYMq%2FwMIABAAGgw2Mzc0MjMxODM4MDUiDATW8e2Xj59h7zk4nircA2Yub6%2FDeww1MvwRAxxJvGRF%2F6hD8aR%2F9qBPYxyl%2FxAv2cHgYiubGXy1mfLG5xq97lafqC88KSfmI3ailg%2FJ2cjCzJoEEp3BT8bjb6SF2WU6eta5f7CL4cIu8OPxTEU59Tp9wgWe8Wv%2B8%2FFufLAiVq2L%2FZkFQ%2B3R963kpnCamJGj01%2F6xb8sZwH8EqnH5MRGgB93uAmyCmjUeVAfBUrxJShUQRTgl1qi7z5LXY52Z5upM08zG9qN8C%2F8k32d6fQiTToSrj07bbkOosp4j3jbL88rRMF9WNlOUVRoCSnuJAc1df2LXy9R0fiLAzmtb%2B33SdO%2FQSqsNFCEFnckw%2Bsb49us9TYH0QXC4EKmimKAXeBkYmXXk8gd3cX9oHwc6rl3unGvd0lFnPlCzGQBFaDIFeiZJLPUaP7AjZvDo46Ody2p6RP9q6S%2FDs7J5yIRyT8%2FYZQdCG4VPZrMj%2Fh1i3sdI9mcWHFkRv%2BHZbr%2FURJ%2BdAcfVQvtj6IbfEVHgf1fgCrV8CaNo9pE8ysaw%2FT0%2FVyU5aPZpCEay3ARs612itmwruM71gSOViejV0AqlhwW9CN68A8iK5DR7erzPYAe2u0jR7MBChfR1rAmreAl9mNsV%2BQwpgvqguA3Li6YncGHMP2YxsgGOqUBareyLUKrY1JR2%2BKL5%2FEbSnNGsnma8DKl3%2FHX1wR6471epWpQmxDLDa5v68YxDXZmeIs0S0Q%2Fl5Gewn8wkLh2JqD9mvWn0YXzfBSZl5%2FSi%2BX5YF9l6XwA9B8hOE%2FQDkE9nj5j1Nun5S9L5C03aPffPmwAzX%2FG5NBKX8lKqWz80HDV%2B3HFKzC19aGA5RLG3Gk%2Fd9g51w8Zwl2LwIz8Ju3W5YbyDo3o&X-Amz-Signature=3cbe9e9b8969e6f40c3c994a77be465d22351bc1d98b4e7ec6bf089d6a911d0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
