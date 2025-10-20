---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKDPVZWX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIA2dl%2FR4fd50eexECqcYW1jU9sNNlunq7dLvA3Qoczf3AiB5gA5CPzLb67Uqd8XougSbq28kdn%2B86UFlce%2BPplPNYCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqHWJ8wroxg8%2Fm3iqKtwDHoQsfs95dLQyPfaVR8%2BWTQCjM6fVdk7YP0hvEduC7iykK58xuXq81iGSQDoQnLvg8%2FartrMJgSqLWkWj9JJR423h17zquY2n7vGUz6nBFqXgIiumR1fk2S35qFsJZET%2FhxDOGr9lpcaDNuugm9u%2FchaBjLUUsgODG%2Bs0z42bRit3cFd%2BWaSLS%2B%2FRTDQGFf1f7xvXfnGM5lZAFlAbfhGCaHHCZhf6rPleLE9TqSZK7XhSUFYl3ZKtYUYviNkT9nlAHVBSLLHnCm%2FgyY34YHEjhdvQ2bSpgr869FZqhxRnjj0CRlNMaYkXjPZK2%2B6t7WZ0y54WY9WCgm9I7Fi3jLN9u6YcbJKuihIK3LijtjmETtqGTTJOg3cf9EezJerLvqwuj68CDk%2FsIvLNwtDU%2FXdrIvGbF0jmPQ8qaCSU9swCs2jMcCmVXyGqEz2o%2B6Skw9BmXXedRgoEhgnEWlxq23LumK%2BZbK6XpuuehAr2235C%2Fy8l2KUJwtBZSTdLB0BBEvXQfzEzaDFCnj7u3%2FYC8yLzN6vCL7wvbsrLGiKcJe50468mo%2Fp5uLGNlf4ogpcvvrSh%2BblFXVWdE0stupECfGAHub%2FqOXQgDJ2kYC%2BmqN2OhaRmlFAkx2av%2BE17hPkwy%2FrVxwY6pgEQUl%2F7tSwLY3EYeTkMQlHIgpIsFgVi7Qkv%2FmX1PCsuSuqUpfR2vnWV0HVdF6%2FCetgqI4pH2qGqRd%2BXR4AB1IP1ajwyRGO6GQlbPUkD1XeuIedw4lj2WPdr87gILJnsLa5NAbx%2FxSrYr9xMxVClTQqVX4%2BpGE8Fvb4090hiwHFZMMZBp3cxnfuxE%2F0BcAfZ%2BBNjYLI24OqBmcSW4I9%2FDwLhICboSLkV&X-Amz-Signature=bd16254cf973ef550d57bca0c04b2b1bd79cd5ae178dbcc30823671c65c1f23e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
