---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJCNDLKI%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T140126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIH87l4k9CBKtzNVYIZrkg%2FYjJO7NK4ymVJBIPn41SNOzAiANH4yPHCygXpOlzk%2B%2FIEntvQBTqcT%2F2cLR7%2F9grIIkVCqIBAjW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOW3qRN%2BCzz6h3MHUKtwDmi9JTO%2FhBrz%2FLqQ7NIakWLVSVBBXBQl35WPya23hXIzTSYmwjFEzXSZ81luOQIDXdywORUQhfXPoqY3XLURYqLaBqyKe6FpCqfNyqqQGmSUXatvzkFSnycjh4pZZBnvfQpzUaCWkmHNFm8FCNYk4Fo8W0duPi55ezxkd5C5rKJJ7I4bxxycv7MGpiGkNcVj2Wxt%2BRx%2FGXpwQC9CYb0npGiZYdGq8zA8nKuFMsNHO1XgnDDpnsX9HWNO0rgSb2snsxNwAtaom4EC10VJ4Tw%2FMdYqMr3Vejk4YDxEbB3o3H9A7iu9TYxWSPVqdpf2J8QBw13hKYf5J0d3Tuyjhv7PHRqbr6hkhIwHK4h9J2Nr3KM%2BPBDjpw1YMfNF3A%2FvFGsbFP7c0IsM5kXmRzQEoft3klq6fhDdJTks%2FDvuvrIekR6FqT6kP7DImv632PeBVtNTWwzK9tuiOOfuT4WcInTsgeWg4BSafKmlyh1ekpDnSGFcMi5DbS8uNTXUD6X2RM56SYeZWgkW92iN26kn99c8FfbZ1PmnlIa3uX0%2FYww%2BKMyEg%2F0A0KzJYDigO8wy1jzQ5pn7%2FvoA62zkPeVugfq4h%2Bdv18IEyUfHQUfCgPtR%2FySr6DZdqeRyVAhW4hrkwyYXqxgY6pgFJ7XAbqKFag2Dhp25ht%2Ba98bDWfYPjKRvL%2BhMeNlnU6mJhc5Eznqk5vq0OU63b%2FdwtQzBWNta6nopL3Ht%2BADSfYalPiLNxWUxXRr%2F90I9fXK1maJjGRBNnYMGeQSdfKtJZ2DMyfpkiS%2BX1zQxNPTCd5PhTqpJESgRBS55YmzkCwNEeKfkQMxSn4nj3FLxm%2FlUoMpcBi0pkOG8ge%2FertIoZ31QN%2FW5j&X-Amz-Signature=673a2d0206e5098b5db5c90e498447846727834dbf4f8cf1abda05731d7bcba8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
