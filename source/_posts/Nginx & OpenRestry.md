---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZE5DCK7B%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T070049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCa514yzjJuF17DHCLGZTYnB87no6ZUh9n8lCiqNdsukAIgTnkOE27zqbDSCv2zZtWgUzqkKyANJlOX1UUO%2BqNZjVYqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAB%2FAqSRUEQDlUagzircAyjmidyCnv9g2HeOcAyaBYsmzUdHck2%2FH0SehEVcwyT28BVxKcHsHl3IsWKqa6ofZH3Q%2FyMrmgy%2FqvB%2B35H8frzICPz8vtr%2Bh4fWJue8tM%2BdCc5SnNqpixL8MJaxjZTOdtEF%2F7duc49bLQfEF3nzojJDX%2BvuF89c52Y31Ysn9lh3%2B2uBRbvQTmtUAJI%2BLW6WuFYZP6GvQDdR2e28tvm3DXQwF4Sn%2FnKr9aSTXJcs3bum3IpWzWSDE2EzinUTNz1nPnRoi6Ojdx4X%2B4WXGqDJAWxi6mMBEq7MS2pYtrxrxvjRcz0czZfhQovMK7%2Bn5W0qbGVCsQ0fJZX%2FHhQJrOLNNZoELRlWs2qNM7txYAtdpY7pFw%2BKd1h2SB5Q8Nd7Zxd7F4bk4Ea2ztmgs%2BX8q%2FMWsAOt3gdPpFL7mCIhcT%2FE%2BUPkdnk4HFq4dYUB7Hq%2BU2Kr6kLqv3QzTHi5KkI18F4TpUKbPOejMe1lc%2FE1KKYGTS2KINFy4YOpSy2ADRi09gCWz5uPFFw1%2FJbn8LJWVY6%2FpLlJt6V49sZlyRXsF7o4xGtSA9Wt427xOj%2FbCw8SvcvlyOqYRH8LCNp4RFQGTOl1Dd5XfzSXJbEuzfPef5SBGpYMzTzJ0aWJpQx4br6MMLjBs8YGOqUBwdE%2Fnf3j4mnlKNzgLMmJyvMxgcgbb2D%2B1MC2IdaUV8EeNeEV8lbdbi0BRltI%2FT8LZnvUfUMfhgtZ9Rjril5qR7xmkl16KvZKrojd0wj1ss3EdUzJ2ZjIzCRvOneSK5ZKyfusCUNS1MytNzRXxIdtFNPeQkTu2825iGgqU51ZouDUD2zGxc%2F2SBRjNmN7Rz0xFvjrZib5Y8yYOxn1eESDNRmau92C&X-Amz-Signature=37ea1eab8a20ec9e487bb2d79f6c9134edd8b1be1d498848e8f6c74046ee1911&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
