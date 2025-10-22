---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653EQ4NUI%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJHMEUCIDNCl1cgBQcczrZsQ%2F1D3MOrBSYMGQDjVV5OK4lnsNqXAiEA5iwqpyBmLvPaQSHjqmZrlvI6VJU9dxv%2B%2Fo9wtwZheOwq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAcaMlIUc2oVeMBInCrcAy9uZKDqCpfRgaeVTBybyXnD0DxwKZPphMZ90ATO96bf0wh1LOkaTlIDPXTMsJb62k5qEna64VdUq4uUBODA%2FW2Uhu310%2FjhL5o7OrhF%2BeSDSZsxUTAbKPskJze5QdSeVprhINFa8zyoJ40ZsSSE0dNcpx84WVNmmjuMxaPuZq9iUJzqqePr539UuPOk6uCGEOsY6ZuwY2JYhE26RrQ2xSWp%2FIpAgA9QA%2FPwebHIrL%2BDiiHFMSmOXkfh%2F18ZsRKBMWbyKalOS2Xki%2FVZ2YNwR%2BHCa8nRrOdFWAaPi1gpDQyXf9V6boVWN%2FSZZcFZv3IcPr1g4zng3W3XDrKUB972zHWV85D5Gfdi9yLvJPEBkdFmM5hH0%2Bkvq6SYx8oA8p5amWLXXbEPWKirWYj63DseMOnHOMEVTW%2Bv4PDYvp0eC1xugUSy5QZYzC8iyEoGKBKaVv5jhXSBwgk6lsLEGpAYuHqX2nCeZ7P4I128BuhB7vm3w0nsa39QntPgVyDuKJTpmhNq%2BrbWGNqCGy9qeZzJk0qTSSopVzAHfJ35QKPU4IEyvXkaCT1j9m6jbS9z3BsMcJcQHKstV71ZtDea3dDp5HIH6nzlWmcygwOODOD1WKvmDVQnq8wBUFW1K730MM%2BW5McGOqUBxKe5WcBjmNMf6EZXHHsjxJhb8bnF24lyI7mMKb7MJzkF5Y0SvDJ5a8FZkXZffvouqimGzpt925%2BbwWzlayI75g2m5GZYQebpCHI9EmVpzn0IKGW1Rv%2Be49G%2FJgYBg6JgEPw01fIPnBh%2BH4%2FNFXBWjxotrlsGz2Y3dak7B1KEm1K7AFJBujLCYawIdHDhUOGiYM3lTvqA3IGzbjlkByv1hECJCjMM&X-Amz-Signature=427df9b765cf30894b77094b08668828c72ad7d227f3fa26188c7fbf81d27f51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
