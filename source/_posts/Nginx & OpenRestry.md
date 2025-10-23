---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMZE6VG%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDPQ6AEtmof3z%2BCQWS7DlCr05fS3fqB%2B0AzB0pGPzlkLgIhALNOTiPo1ayvngwVilWRRwnd0Vh3JLVBUENa6TQD9vfqKv8DCEQQABoMNjM3NDIzMTgzODA1Igx23GSK0JK4VWs48sMq3AMI7rDDdUe7LXZnuRNVb9HL9e49GXLwgbhToXZqaSqGIWHyjJt45DD25lfXuxyCy7EjT25KxWkkuh0BiyAev3QCh%2B0bq%2FMhqJQwQZiux52mX6k9Ytl99vOzfOwolKtm2DI4oInx9992MR8orYTBVsRaKxmwBcgqtpIBSDi0KncWfNfe9As38qXm3n8ATUWV7nMIBTeCccYi6uSODNfotHLzxLhzKbhoE5l2XzEPtU8tQXppY4fRntCyl9jJw76c3Cp3O%2FQVQ5HfjCb8YsPy1yS72VwpyAixO41Z3Q1NxtH%2BraeW5nBlLFbmS7vJc28tUI8F72x2K63oZ5aI34P9dRcSAc6l%2FRAWs%2BdxxziLWw2JjuVCYWONE3GV2eu8XGis6aRe4jRvuaFaz7l1TrQLPJf4aXF%2FdVNK%2F0BE7uZPXaYEjelEJ06CkBM8oC5kMQJN%2BqEkakniNoiVL3VP%2BeicXJwMqqNBy7ZRFdVqOj2IJ6rkwk9cuQkNEjrbwh5G5RM1sjJzPzFai9Ll76zeegcL%2BfuBwqKRKR%2FEZPFP7e5t4PLJZe0nQIELiEFOXl2%2F8wQLvqw11qVI%2FSDldKUBX%2FgNE6ynzkLihjsL2yQDgFH4NNj5opJmoqPwyiV7epogoDD1l%2BjHBjqkARyewTPkSiSf12QxqaV%2BuEBvpBR%2F7GGt824VAxYJOWtsvVhENIPhuqd9u9uLSTt7iy3%2BzJ4jO2DXVR%2BJAbNT%2FDGLPF2c4Q5q91OKZds76CXFBF%2FYGiXW0rhIS3UvVBJik2xmFIvwPCRa1XmL5uEPvGazihLf9wVAGjqorPehXimThDpTn5X%2BkFI9O0HTdJWCs5RJuS%2Fng08hco0bfCokE7i5Lpqu&X-Amz-Signature=d8135e4af6cc4e0b150b1b7c31820ec453cd120bd19bacf6c2b035b898dd4642&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
