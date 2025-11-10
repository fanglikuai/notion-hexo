---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBGN3DOI%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDLxSzgAdeSCJStTMx4BQXcWQIpII%2BxJJ5DdFqYxUy%2BsAIhAOSKMNA0JMFm4EMZzZcp%2BzErwwp2kquwD5Uh%2BHDGS1aUKv8DCAIQABoMNjM3NDIzMTgzODA1Igx2qX30h96Wd%2BaAsHUq3APGUq6aiEJsiTcFI7%2FEYhYi0TV5CPblZpF1bWbL9jEgUEavh9zHlaPbe%2B3gsV4v1T7%2BFZzOr13bNyk65v4FXfNLj%2FDARxWh9Dhey%2BSuvA3OwuC3ymbRUWsLqRX9p2ba0D76MaY2wMUB0BSLxhGCFscx8jlWdLM%2FI3Mb7U4ZmfJXebMQp3Jxlu%2FhoEELzSkjOPpN%2Fuj28z846rqev%2B%2BhA82DNowXggr2SXL6qqNYwTk5Tge7dCoV%2BqCUuaP7m20DG%2BDnFDYn%2B6PAzjpCHbx%2ByLSY4Kwc9Fuu0G%2BkAxBig9WKRNnbA%2FZfUPX1uzAj6zrBOpzHmajlkH35n%2BQtbvtZvjoOqH1qmaX2zQa%2B1ZZYLv4edag5AN1o%2BIyZ%2FCGrt9FVBqk2yG7zKacTn8TFvrkmZ48VHhKmJS1T1ex6vGo5CCDy1XYjE1yadgbMzf7fu9zHGIzLORp4dGbC6GpTdi6FfeQpV2sw8aIOmVaRHHGdCpdb%2BIL1Rv4GWgC9M1r61f8Kg9pCqGHfaWNP%2FVxBcnx1Rq5YjgzJw%2Bgwxrjnl88bZe1lkyE49brIwYM%2F8FWvae6y92MdsvLa77XY%2B3gVT3B0xzG9CXtJzhG6SjDEgaZtKK3TR%2BNzcX4M5uoNHeX6XjDY1sbIBjqkAfbuIASCcxsRHGuCztfVEKnE4Xb9HX6iimsHEXPmJ%2BZjYX043ALX2ekkUlyMX%2FkVm7UHelUgVWTLNR6Yibx1iZNDKROQ8oDCpocMfmqjo2su2pLTjnDcVTACnp4BY7KzeUJ8w5kGNmXiCYDoWC6lScQLSMb2vsH9STMAlEM3JOVItKETzQEwmmJva7uNUzqXMR02jXrhh6nc6EoYc4m5HTO2GUop&X-Amz-Signature=42c24d494fbaffd0d4c2e282b408293f7041800034f210f5b884adef2f5af297&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
