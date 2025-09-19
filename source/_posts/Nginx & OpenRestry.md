---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S6WBGXAB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T030108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQCyj32h%2BacCdU2JLLnnqf40RTIh%2FPIuJ%2BkNd%2Br3cYn6UAIgT2KQbcegNmHbUtLTx1Ocb214tRcZuag51DDr3e%2B4tlcqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIr%2BfmYuyU1wZako1ircA2152UWJn%2FUmrIunjm6er2A%2Bq0L4kJb6EwKDMLeeDvVyy4l301qhnF0zzTh5AoA59pu9%2Fg52Jbi83YEVAoFODsjJ3Fo%2FhqgCsKufSeux46fRrPfshiJz1TI%2BlrvULDKbxtX4oCzb7tr9490Rwt3Ll%2FD%2FmLiyJjCdsL6rcsoFac28xLESYBb%2FxHYAM0eP1hlceQMpPqoI7VpW2R5Sb%2FwLP4BGGS5trFjValy3OiMh8hW%2BnpIFLfyFRkaT2QcnbC9Eck%2BcjnanEA1GPX%2F8GUwZfzrxf3SKG3CUMMOIrnmr4bDL4wok3EJFExcOELri3NfDANkfGNOAbAtD9Xfljs5jM2DptMJTSqzlLo8SAcwmtt1Hcl487VEFR8stZU2WJP9iIzbqqaaj3fGAWf8cDiIiUD%2BsJdBOO4RNLxo4H239Qsg4Q6lBRq99aCP5KFehsWSAmUdJduoBczUTx8pKsF%2FQgQenOsYiaUhICNyf3FYw8%2FQsr5t29e39Lk7rfNzJUFVqx8fxd8obRkV4Jn%2Ba2k%2F%2FUa50%2FOaqBj0qlVuJ6xbEQR%2B8EJWEZjrQyRmJMGtPBnUl6TW6QGCJFh8Aegv7MqSXjMznsqAjoJNiih1NR2%2FyNx8EOhIUmqKAd01t0qUdMIWgssYGOqUBtMBsxygJpwCbsiIVKh%2BUt6rRbS%2FF4Dl7nMO1WIj%2BGNca%2Filb2gaL0alFWveNUOa0ugAwxuHhV817QInNw6sMEVNvGrqnDkp1WL3gWMia1w%2FqhSdYd8QJeuLPlJysk5z4ROGYCCd7tbkcdJF%2BRnJnLI%2BnP73jxnqjS%2Fe4dN02CN8K%2BJQyhKh6blV8uIibo1enRVTfyhgkDTLUTVRrWGz0nEBc0StR&X-Amz-Signature=152279aa4626709c2fd1029ecc43ea5174535225dfb4d5804444ce3ef08e64ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
