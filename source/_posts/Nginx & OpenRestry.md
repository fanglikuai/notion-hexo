---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFUTZONS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEM7TFJEc3xPydEo0u%2FmhhSewUt5ey7VZsXMuoMH6snQIgSNp7D5YMn8llp8ibsa%2Fuy%2FHeEKJOViovGTh8MNzHLqgq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDF317Kcy2JSgFf3FNyrcA1GBaxGF7RRsC6oiQDofn08Xbrkac3LbDhEkgbw7R8%2B29buAY6jRGkorTXUIxDDouLt1RQmG77PGrI4raBXV9YTMPyRFp7FQEGNOBOF95hGOstuYmemctaZ0%2Fc7HyXlrDfiS5HuwUm23RrZ%2Bs5la0ecKK2Ha6FMJ3G%2Ba19o%2FqV1w3u9DTlhHbaSJcaZDBxa73QQAC4xlomx%2FkzbaZ0okOScW9cwsuOIO12ONMSvjji%2BtlLkwVXracBFiVK0zsAyg3nr55i5Mx5ENwg9XfnUHjqQi7MWxAe3X1Yk0t4J4LAONr7t0AYq9Gnl5YzKSV7SA4NSIohynl4RYCxudOMUyDWWHOawclGAi6l1u0KC%2F40ZQkM1AhYCxfcF82uPaYlweJigDNbikl%2FeZ7WeKvRbDclG9%2Fe0wrgMDGprHiTeKmLtb7FSEimEA15pIR4XJ1P5IjZHsMEhRCyrtkdhspgxL3PHKLMvZpMh8nGGnEncfCXIPg2C86h%2Fgm3SX0ORHujbjXy4aoPbsv0pFa1gfg6xtH%2FaCNkoHLiZHrIkxgQJVV%2FlidfUqKwCxrLke%2FFhJ%2FR3TJiA8B9Yp888ilVFxYx%2FXmStv8UYbvJnfngw2Kiju3uO8VjhAfQQ4J18ScTNnMMKwwsYGOqUBavTQU61VIyseqgFfBhwUhgAjv%2FS%2Byv%2BFlGSED2SvuP8QSrSGGNWCAVfy%2FENsCmooN4NTf1BLc4IPalWR9loNi%2BWFDMSP4%2F9lQZspP5zmMCbf1z%2FULLHdVdU1PFwDpMy8pNsHthmJtUtAAKZ25wfmNWo7RrUfMigftT0e1NgD8OQmBzxvXWijZiRcSD7RKpxFxbfTEiVHczxrylJqv9huHjhdhOcK&X-Amz-Signature=6fc6aaa624ec70d09b3cfde615461234b44313291365863f9be76871a4f490da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
