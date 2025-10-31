---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UJBX6VS%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQCis3Brk4IshlAQqwndtIvjKBWpWeHOnvIWehiqCyMJNgIhAKcTkZQnajxACRfkjx00ryi1%2F29VJiKDNKYnHv9gxancKv8DCB8QABoMNjM3NDIzMTgzODA1IgyTeGSy7f6mgnZbIMAq3AOhIYpgkHR78%2BaEBBGi%2FD2jjP2GBGIGLq3ssGUwhV6WvlA6n%2B6wFSreilwg6n9TGa6vTKK5Q05p17dIhzmBsXgy%2BlUy5ekyL7VO00dDsptfEMXjiQUsPdpiN4G5zDS6TvHOZ6Lu1i5od68P8VGpf9NJ4tkRVkr8LHtsTxRN%2F3%2BD1NxADY5N8AeWGZhw9PRDhts9%2B4TC8Pe4h6uDclo%2FTn%2BbcZ5weyQrw%2B9dKPTgGPSyRI0Fkp9N%2FGekB0atpdrOVNJoOvmRvejo5S03ePjRMh5YUzh%2B%2FE%2Bh9vbaQlL1WlMmWQMhm%2F3BKycyRw0miOHvgFxts9U8jU1j5wxqVocqdlKu%2FUAzopVTnG4aDiiS3Bjx%2Bo9wdcQsWsmQ1eRN4zIBAHIYsCoAMWZZd%2FKZ4PDK4G7kU3Cevzyi9jd4R0m67x91gDya%2FmsgjbcuvwDNJGz85zpKnQ21cy3PpiOe7QASz3bDXgt1eTLd1%2BB1hbglS8Le%2BjZWU3UiwqxxWmr2X3yZpWJcKCQjpjHdJsf%2FWcZdXx%2FPPoLnDLylnI7wWIXAy20NZ%2BV91h2jQgkOb3jnJNZNhoM8ds3bvpP9z4yqkagblNOSKmZxXPvEgcZgrKqEGmEqvR8Ci35bcGaF7R4uUTDK45TIBjqkAWI%2FfEv92w210RgkThP47KFrKxHfxXzQD0uAhGswPnHjnqoKwY1t2wWuaRcJlwucoZSSR%2FwCBn2w3jAkGr%2F%2F6ZYJqqDwpW0BRx10t%2FNZDKVc2wfJFULxcf8l6MrH3h3Kqu3xCeDXdDKbzZzx%2B5Bwabu0Je1ESrJx%2B5OAMjj7YArdn3W4YPH8XQ%2BRexwae1%2F7ZrgZJmRQGAZ0zPSGMxDMuw6AjY5F&X-Amz-Signature=d6684ca1dbd859c738f52fb5fdd290c57c9a43405f093152c994af0804eb708e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
