---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWBVD5OB%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBfVoPJ3QVeQ5cLv81d1cT2cYgLrVD9I%2BoKGYEtSTMWNAiEAvJ3B%2FH262brw1mLPy2Uz84%2FJR9QqWPiEdFkO1QsM0aEqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDECaz5Upay1jv8MnGyrcA%2BDXmzFXC8NuCSFavyvahkaJhvHltB0a2yEQqqNwFgMxrE%2F3b6DD6xW9PmlE5KPfiaJC%2BQP%2BIlTiPuW%2FcLLxs7I7O9WMAWLRTuU17fBsAuVH%2Fz2WwQWROs26Bj%2FCQ0XqwUJLFyliL7Nskp%2FLL00HfUYmSVf3YFiNxb0V304F02DM3EMXA%2BZcWKUbq6uyzk4Pov8iYFNqnT9O9hBsdsAfOFqgr2zeKLNPxWMyAMHt1df0DB8Q5UT6fKzTH0UeVpe1F%2FSVYknllmJJQDRAzT%2F7%2BAsDItm3gH8cJJQZgURMxKk7KD1fs8Dk5G%2B7h2cUnvfSTSsapolYo25bAiVAWZusvm%2Fbs9qTnmO9HwwiAYxnMtnNeVUj3KbC45deEXWqoojhUAgSrDOE997LFjV%2FvYEa%2BUp7gRTLbnYX7O0IUEkWMVMcHqxVxqTwNwKTn76QJWDyJIWhfck4K1lIzjmJ9GeGcjclo7BPCdsRjPjSqFmXOu65n9m29pCIcc2IDyH1%2Fpe%2FI%2FNwJK6cKATzTQfpbtPqAR62MUpCXMUqyGVOQiiaEU5xqU7ATxzoSl%2B5%2B5nhKY%2BK7PMZa5z5uA5h1l72%2F30BqLxtEvK%2FUonjnJzyvTmDLBzMImMCVbhamNVhCXpYMOrJ7MgGOqUBRghCzS9OjpqRDBkIW%2Fcfz3cicSnvIv034QO44b1ABrYqX%2B9003PgYptGRSJ2gxcp3fzENzf5ByaGDn%2Fjc4xz6B%2FraVSyDYN8%2BgHN3Ug%2FABUIDrnP0CmUmZXIjb740droiDKlOg591s4ND0QbbobclAIPq%2BoqZgWkBnT2evga0abHL4v0LWDn0PqWqT03MV%2BBiVn4hmmf4HWV4qGNcj%2Bpshj%2F2Wbq&X-Amz-Signature=c9f580e83c4115e01f113166a764fc513a9d6b48ee2fe23a18d0e538c6f06273&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
