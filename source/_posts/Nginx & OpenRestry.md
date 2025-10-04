---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6KHIN7P%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqugIpWJwb8OmuHLXLDXf9CwqX3GOmejYJP%2BUk1k2FJAiBkIMyDuYbJ1QxiV6LFKqA30Am7hDAlYCYLu2LrcXJ5hyr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMtOmmmcdqbDCw%2BQrEKtwDL12RhnKlL0Zy%2B7urnktujJyidVei6W6VXZ7IVHq1klalseCQs2RGyI3qHnBxBqtJBHVtAoDUfVQWZq2GEB40O0z5frN%2Bsl9gEADVzXCjHsv4f8HSbWfBVoLZnOgBA%2BtiqTM18llIO5dCL7fUEMXTTStZMIbVuEaLZfx1UGcpwmIrmBSjVW91ztIu0VfL4A9gC4F3bZChewWcQfk8WLZp9uNmYvpvEOgH%2FGyutScYYsoh8k9KGC6El0LOxUV5OXLn1WGmasOOQI0JjQjm6CEmFBWa0OF4bRasdbixDclI6tnmgCcB611romnl1pg423N4bRxcnFhfKJ%2FLFzd7y8nQ54S%2FKY9%2FZzabAH%2FhEAwJH0jW4pPXftElFwhwLhmywetFcIHwxMgyY0565344ugs1ZFfby7zSAs09FOvjtmN5yDjUi9e%2BcHiilGFmHraiy24YNvTQcSp4H%2F7%2BRp91hJkR48%2BPbAkNqQMfdpRE4Pxp9m7DsPqdDiB9h%2FlTNM28wE09tIPh%2B1O1hV%2BNYdinELT%2B3ib%2FU1PCSphIZsZLvuejqPhjPAMJR%2FoeNfs54%2BAkRZDknUUHU1jH3wS%2Fl5BX6ZGZ6zjUwS%2B4G8mA%2B2RTC9nuIIlEKLYjybneKHn5tIowgpCFxwY6pgFMoCOgOXBUclaLg%2FqIzhRtxUtYUoh7npIUBgDiI%2BrQBULSnmZSObR%2B5w8ud0FKBx0Omzf3iPNxtDR3rF%2FDhtIGQHxdEB8Awhag8%2BjXxDNP3IdmWpCI8TDys19mBKla38%2FYEwKfhaGneJxaR3jAOKxvByfcbWKtQNlWiK%2B1p96c5xh2iQCyz0n9jar%2FnEkF%2FznOfrWLSC4n0ugx6yfgiS0IWGSv2iEw&X-Amz-Signature=f9f337bf82c03df48cc290fb0a3e8fc192d4b70bef657b1839f62f115e752cca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
