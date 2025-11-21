---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCRTND6C%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIFDHCUQPqQOtLdwfIVdxU0kyA1rXS0p9L8EQhX%2FLKcJPAiBWb6taLUVrbdoJQeX2eqKztQ4KITLEy8hgs0GExdcHJir%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM8s2YuJ517uoaJis7KtwD1eRb0I7irvgORz1e1uXQdNSL4cCEEur1RFh243fHmai081ZOISlekoaIZ9WiKpDsEJ1pWZNt73g%2By7wTowbK1cDabCg4RXUBLHk45wcmSLl3iXxjVZMc9ljBN1fKLRj0H3gw743fA4aZKDUCEt5p7f8KoCGKsdYkm4hehm0rwQSr4C3dRiaeZ9%2BgrXZpG8sSb2DNTq2UZj3so7aZ1jSEBKSp79OfdqhAHjX4%2FWUh9keQwS3BmFu85Vx8hJbvaEXWL5jTbB2yuiJ3eAMRgkqzsfnIhTAVO%2BJOL%2FqL2wHtXtns94PPkMAc1zVmJFgumET2iY55BndsoWvxBsEaIz5JhgTMKNIlimk%2BYky9ZXML88VpV3if4gkARjooN%2FqPQdYQnFBUjMH9YYxWao29KzFCqpnIR8DzMUMzTJxRDUamTPd0FKuUbKtqvnumhRb9T46pk1%2F%2F4wFQHRLZCE4pRIuxpkBhXpwNwgM1PZxNj2fTJVf4Oa27yTGkEEu03ZLCR51fR8pBwas3sdGBPXo56k5xsiJK1mzvLDjo1fEAKei60U5QsRxym8Gz2tTfF77DgFJSbYYmw2LY0BRqsadmA9RYkfFKzI%2BUU8lXnJtyjwHphIuZwEd6kjK6oKX5Fsgw1MuDyQY6pgGcnpCoTWlL30cv2QfOf1VjsmpeJKXLGkhtQRpbztSZDknsd9BheRitqAXX1i7WELu9TI6XTEJGKzBHa5Y4kryd2cZNlaBT8HvHvbHmpMICAwX8AAdy7ocE3GGVrFxqXO6w62ebI2BB9EvsPM%2FKh6Gm%2BWu%2BVz1zcpjtt46ZCloQUDrpmZSbo8%2ByP3N6x19UnovHGy3cGaz2iRgxkNeAUYk9cxEpp30e&X-Amz-Signature=b69a8ee5ed00529aae10e2c2ccc6f30028642075695cc79d6c9ac4438c701f18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
