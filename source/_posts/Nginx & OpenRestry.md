---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIZFTQE7%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD4SMhQECTyllu%2BJfJ1lmS6lRka9JWea8kIOnP7QsKskQIgQNzTw8WfHYB6k65GgsS1kvrLKYZNK%2F%2F5ZPv7PVvaeGQqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFcHS9pCeFEo8tMRwircAx%2F46xMZp11WfmblAB7pS1nQM32XQ%2BJeJmWymtZn%2FX10PaHG8pcysr4IcW0Vnlqnngiu%2FIapIZO6b4Vd02PixGzzCl9kdcIIIVIlXtosAy9OMcX8EfivUmQmT%2F%2BuQ8C0sR1ckQMqaP2PR0XVT4ZTTh6JMG5Nze2XC6P3fj9eHKZR86qFuWCNzuu6uB%2Fhl7DN8nPdFGzcDkpiMYVFQ3MqdN1Qs47baysLbKE6vCK3jPCtLaSHsODjLniKZ5fOPhMF3PIzY4UBvFoZvwGCeINhsH%2BfJtcmn2l2kAtkMAsDI%2FvGbGDbcCGq5ru4P5SyS1WrM%2B2vdSIs%2BBqJQALaLCbxlviltpkyNsrSmcVtlwpppoIxvOze%2Fx3HtZmPO0Nb%2B%2FTwPID9uIf9SxLCUWZQvpOos3DD52aORiQgwILQnJtwMUwvsao9peK8Ng94wVoHCDBIl9fnE%2Fz9glPQasI4f%2BEsOsfpTkdSVlGz5nBKNboZd9j8oicvGAoUlRtrYLdSW2RwtoPQZSmmzq0yfis1A4vb%2BdGcuUgJpsHT0VXkVOpsKseoT%2FXlU9CY1rA%2BCY4Q66WYtpfweAknbRBKOPF02%2FB7ChgwXeHa0YlJSQwW2f6RNak5L8dQCTiDmILrfYKFMPDv9ccGOqUBGEJZ4TG0I0llbJmcj3tLyTGnFqPqIsvI3r6yV5Eov5Q%2BX7wlsib4cwt4Bn1HqHpuY20Kru5SydzcXFpsoiOCjD9sXfT7EXnaG6O47AcuHdS9iNA9P2Fuh2vOsS%2BOMT4TaMq6QSNmg2jGln2p%2BKXsmxzoMTK3CpFl4wlGYfq5V7r336BZDP3jB%2F0AQpjWNSsoLX0KDivi9P%2BOpPa50lbKDtRU3ZAw&X-Amz-Signature=332e81d9674640a8a0913bd8dd854d8aaa8fbd9d85bac71086ea1e72aff870e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
