---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UALPTGLH%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T170123Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWZVekGJlG2g8PYKsUnMaO7waf0gtQPAM0dOLyfdjFLAIgJgPziuyqv%2Fkyeq8Mxz0cDwM3BV1yKj%2ByvxVylhM59f0q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDFq19cXEd96XKMzPvyrcA%2F2%2FSY0Dc%2BzFjnWQsij%2FSvb5vW3oTn9NZuvTGYnh5r3bIYh7VO9qJ6rlvuq1hv5byjN%2FOqef9f3%2FL1ZKELZOD6HsEx03Iu0mlowF5glCt7IbasAyTLHJmcq5q5XK8mFH7GOiFuDKQIVlM2FWa4TfwLBq6BwhgcOjyf0MUxtqiUs4XiE4lMSbUK%2BjPgt4mJPfb3eUN%2BmpeNfY2qROJRln5QcJTHzRrOQC%2BbTvhfI8TBeWWPza%2FtTpSpmbY2ACRzWZOnSr%2B%2F%2Bdh%2FYqBWmzrLYyc1%2BXg6zwq64zGRCSj%2B%2BQghCbCAEPoSLsSkfXqQ6cFp5bDW5lcwDsQ%2BheYN1dm19WTXmxzuQcgdKSqK%2F1Si43A%2BbaGuFyWZ1g0m26F2QbPBSDYSC0drtNLM2QaUmWq1tjcIRraF6TJkxYSD46BASoCwQeGxc7yU6eru2zfxNV5yigBj4N7JbRNfhbJKHiS39XOxkSt6lal5P64N%2BkT7z64uHmc4gxts4exm5TSjVEt972ZM%2FsH0VpYhEAGzN2EZtJvWRdkNCKp9XbZOKkyvjgGLKW9NzCPUhKlgFQpap4Z1JJkW0QkabhINHFFQqYZeIvAwxGhz5UF59gtjgJDoTo0mbJFpLZoOu9xPP8fOV0MNWV2MgGOqUBTtsfN%2BQDy9d3gvCNPfANaubd1FsKWeHAorN%2B9rPRL2FILD0y6LdrcksN4HsDCZuCrsG7%2FO1%2FXsBdyQhklBnjUkqZkMCpxlOuxnA2NiDjPrEkRLytvGsZ1VJitDLG3xfdWFGBzRny6fUh7XIpHVihrQM%2BpBSAWE0mKrK02ONg0DEOaAMoqT%2FMtC%2B%2F8uo4L2fqkFCOMzgiSwRM3i4EDFtbYrjFwWxr&X-Amz-Signature=b8a4b0b6b5e825fc81a09e7f38969d34a6365478fc8ad0266a8f54e199a72fd9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
