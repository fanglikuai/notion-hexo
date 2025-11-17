---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QT3LWVBR%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC7PYnf28qqrdAisqC30qQ8gdHxMx3m1vevZKT%2BDAXf6wIhAJfmhKoy3IWG0w02g3rIlZO1kk78Hryr6Lc3pp0P7e44KogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyMtZXREh99Toi0NHkq3APSjNOyf6AYtph%2BAE6t%2F%2F%2B2i3AH4zfuOqrLVbN%2BoYvvS8CKmIKZzVshUfADZhwXxV8b%2B0W2SIeAMlaQW9rnVMGbZ6%2Bid8SX81bYSYOSbEsnH6ce6x9h6RvYwZSn3W4DsC3B1JeZIqaqbZ%2B6fYuu5%2B6Imz%2BIQRmvXe9R4cPPfVobnlueakA8kXKC4MTz9IZLpWZDHe%2B%2Bgu%2F4GQMiUMIq6GsIOwb1bKW9q9NQUSBu3fEj1fxcDIkVLi%2FPHEQM4tl5kAXWJhyejI3w%2B7rc3wDbmQLkytPrPJ%2BTijPRK1urGqi5VTGP1K9uFFIdMk%2BKtbsK0Z2nFqBmfY8KkOn5GuQ7Y%2BHGX2ozlo2e%2Fy2ABMa7MgnCwZ3byTJ07UJy%2FbnrIzzUoFcLEv09KdV%2FmT12uTREOQUOkLwjKTF8AahYW8gi64rb4H0T0pAYKKGOMlq4PsshI4xxolrqU6YAWxgcM5DRh8eXMUWrmPIV3BDKp%2BgNpyRm3KH5bLpMWUPDYeLIACk5MihY0trJsB0BgpfhapcfHARUtfXWnUzMZwhJAtYWJ0lOPzXTNX7JcVLrjMMiDM81oFviJC3DbBjftJCKUSO55ewc8xZ8ZBcuFVc5gVqcpBKnRXgN9ysnnmhqGy5QZzCUwerIBjqkAf98KZkR4fLpJEL4P4M68PKnz9wT4qiDd7vdylQFspC4S8FT%2F5zX5JhN5YJ7y%2FV4CwFXZKida0ZCT3JDT4CCL01Fts%2BdFUgVM2RVKVmTIHUJEpQCXNBmF8Lrgk34HeZpII%2Fz8LmAQpkZQKmfjWKbwixYITo%2BftVEw7LBtOHnRI0QfcEZ%2FN4lvuCnZKOQ2Naw0OihmAF5I08nWTodJ8aYE0MgxHoO&X-Amz-Signature=8833df42f58c2feafe168dd1df5df183849647c9cb972e2a8a2b15975836b61d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
