---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLMZLRUF%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQCEhsMQ0xbTeN3iHjLvJMs%2BRVBxdhCUvuIRjEr6QwUYawIhANK8VjEUM2wsRR31ygRPfeZFSwMW5oivxnq1NGxVwzQ7Kv8DCDEQABoMNjM3NDIzMTgzODA1Igx4p3k%2BLFg%2Bw6X3yNgq3APLBZoSaAYlwlIYWWR6pBABricNjKO%2BGD3KdHcc1xagi6hegivPE93UhKRM01Yt52VQF6VtE3AEaHzzEgFBBlRM6A3%2Bqle%2F%2FgqS3URVxHWa3OP0W8Gq7Zg%2FwTjCZ%2BOVssB04WiGE644wLvTqX%2FnqGo4%2FKeF75nTFQiGvyX0fp9jukv49FzFM83XC5YG5hI07pd%2FTDr3n5hUKr8hQ5c4SYdKXRXziIZ6LFYyCjVi1K2sQdZsguNXeCg3W%2FTjPMZGDe2HVKZ%2F6yyEoXvW4vpwI5mMlrqNDGh0iIxaFi%2BnDRleLIrUXTbBpqlzYQKROWNt7hLcer6wXgnjfoox5xuIClDBSH40qX4CzeoHAc%2FEWudu5w0kzBhNfXqo5RlHfbP52IqJYf6QGAowE0JOqIeIEFbXbrLNR4O2fd%2F%2Fm%2Fz7VTjp5nJT8fhHpZLrnxIE7UssVRRri1khxa%2BvaqnCyOaoMgr%2F%2BLFu9is1JqxKMcZzBmQ%2Bb92AqdvfWlsaAaHQOKHuu%2BeDkqzuzmReXvF9WoyvMWeMvfKuErEi%2FhpDfGDxoDpdTtkAC6GfzKhpEGpfMKit%2BRr%2FodeFm1Bxz8EwJ9I%2FyN0pIM1ex5lUSenD3NNXZ2kTo5YpdI%2BxQV5GC5VeYTC6n4nJBjqkAaAF7KyaigTdXQxe8S5PREPdV47fZFE8HyRLYUT6PiTPEo3MrifkpJcFK2AjVGCjyDBBAs%2FFl4Mx92rStgfOfKp%2B9xsEvQsTz7XBs%2BuoOTMuSZvSsg9Xtt5o571zMtY0wK8Gd%2FwGO%2F8a3ycBeayidjGINQtPa7xyaLSHALbOVRsjeKlVx2mf4%2FkLI%2FIaj7fbcMnLbicT0CxZRs%2F%2BDw2pv3mwt21C&X-Amz-Signature=9e8209fa5655516b37f2e8c706b7c0be09aad1d904f5eedd4ba28d12f03bf0bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
