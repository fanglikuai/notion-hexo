---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSBYPRZN%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCUEeCvJPxhY4bNVtgZM03FMI8LyOR4h2gvxF6Kx102kgIhAJjutMYMK1mSlmbiTvZBb6U4eOC0Ecs%2F8xSosoikcTwRKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxU5yu5wwM7fUcEKhwq3ANseY0XsJim6pi5IK%2FNijVutH1s0UhoS7aMc%2BfLt58iAp9ErMEp4NqpTs5ecUYSsoghCCvxw2QvA8iUeRDkvXSSQ3RBcgxsshMatNI2OYbN4q4z8%2BTOBQaYY7yfyaaUxKnHrEL7tVKdLLIjH1d%2BdMHHLPSWfRr1HYoRkH7aJjI1au8Ak7nPAIKBzbf0t0vtbOwVsIdyVrkNqqisoiIvKXPm0QAe%2B1WCC1DxIsbmS2jT22p%2F5gOAkJQr4AFQZ8P2CVcNmw8UCyKzkvZwie2SJMwT8xbM0wmmtZraoUjqlyn0XILX4nOL0lehV8oFh10xBga2JS0qTS7FuljqszH1BdmV9SFbAVET42tDG5Z2rfVjWW8EZVlYOf9ESovJKdyiR8oKvdzUGMfBY%2F%2BJuXZdiuXETemXBloQsGmNszOkeuCxm7zwj3VHtulA97lgd%2B9fZpEASvw68cUsQW5T5DIh05HwKLI4Ln3n4hnYTtXqjzZZKLAd8CFy7GcchT1HalS4aNZ0BULG2dstgBv5T7XHG2sG45tj7Ai1UAIlHFb3SGTydscaziZ%2FdvFt3oVrU%2FCfS0g8F1tQGtCS%2FlsUtK3x5MgE6SBDoBQBRkbrJXuvIhQjf6hzkzFy3dNISgyj2DCXgYvIBjqkAaBB8AziR52yeYvXjKXd3yp8ECbcPMHkokZdqHztuLabPFHgn0E6n4HtckgRh0e1YihvnTmI8rCObovs3KWTxpGG%2Bg%2FYLZMbD5xcrjVyRYZi2Ww1gQXotu1jacykhwwtdvwVMImdOlIiEgo80JNj3R9ufQCEdsD73hR5YIIkY%2FXY3ojuyD8uAgNmPqDLEhW1VpAPX6duS9I1b8T3u%2Bfn6E6IPYlM&X-Amz-Signature=a793a688118e303370349a4de2cd3aa8643a5b15a5ede672c65073eb47469307&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
