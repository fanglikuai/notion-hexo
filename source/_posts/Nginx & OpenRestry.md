---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WU7NXWA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T230048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDbB1mImacR%2BYp0gkpViz0FZQBKy9K1G3oT9xSU9FDeEQIhAK7dbRbwvl6u1BLDmiJitIhK5PtOFAqAGl3JRNwzU5BRKv8DCE8QABoMNjM3NDIzMTgzODA1IgzRA0ynzAV%2Bwbf%2BX8cq3APlfB6ERQefA3gx5CUHXS10H3g2O913XESGhOfwAc%2F7%2F9YFaEyQ78k8fvtf6bdSM%2FG1A5XhbwBYN46dmd%2FTHc3ZNJ4Ahat%2BGNbU5doBmXoZ%2BjPJmj0OX6Ol59wI2DiOzJ3Z%2BB4Ljn5PB5ng5EZaseI%2FP8WWqdM%2BUKFOjiUzRGvXkcmfHPiHCgrEuW%2FafHhBpFoEh%2BOFLyaaaUuYIAwjOM6F5xeICV0B%2Fw%2BT4m4q%2FATQK0OgJF53fw9JOsdP28efqppqaw5EAJ25Dgvhg%2B%2BqGkJzi4%2BJJfCMcBaFpZzCHraIG3Kkr%2BS7fFGPqdWP1ZjT2KtjKhr3%2Bbcajw8WyG2vN3rcs3CiKoPL3XBEqlUFmJydU94lIlSO4mXJtKxfoqTKmEq6fo1nTst49TFIA%2FXG4MuugTZpk%2BS76UQdshRA%2B1COgiB37pQERJmdQgK%2BrZKxFM4q56WkdrqkAwB9tMpXxZ5%2BzJe3eaWi7nFBxqeOAqbFa%2FWZLJRSEx%2BLbyeu%2FYd7u3k57dF2Syg18vygHujrBhbvXu7vNQgx2hYEDIC2rAOcmFVsH5FH%2BTrpblm06ijzb3xyIaNDN6kZlOaS2kXKWVBFq5fWWnioMf74BIm6rxmIS7efA10WRDmXScSE7TCbyOrHBjqkAea8Mdu92zGJtIk1TNdEw0ykgcQvI8JC%2FtUs3TNOayAGzXatKlelxlyj5nSVOpGzdsBIaiLAcI%2Be7v3gwOaFuGGHjiAvpOtqFI3PijtP6w7Q98GDSmN7G2zYUeOZNUaW7IJILs4Xe4Sjj0tUMYwAklvTieM8tUhXngTvUJ4Fq96d4r1wvjEr7wTpHQee9PMBIKN2E0t%2FJGFXBVNlDz56G370DyNe&X-Amz-Signature=b116197718388d37100a2c73ccba44fa3b7e530f57d3852aed06cccbf6f2040e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
