---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X263LBGX%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTd9LhymLrklMJYCUjGk%2Bv5cbFAibKUcpUhlRsmvJZEAIhALciP%2BwjmAYR5slXSlXIpvW8f4ixiPVf0jO%2FnbU2tXS0Kv8DCFQQABoMNjM3NDIzMTgzODA1IgwgOu35BjkEbCydzwoq3APs0hN2aM5hFufJfuRJQNVR2dPG59a0AShTgy2fBhscSSyQFeUu5uwdQ0pGTTWq8%2FtDwSqMoQy6DIBMbsWPqkAXRkcQXfvvSSBVzNL%2BZzikxvTYZ6RG0LdfO25LjyD4BgzIXCtpTeKfOZuFCBHYbnMURnYwkJjkxZ9tbylyDG6Y13dECWbGq%2FpafaS7Xs%2Fgy%2BxssudRbaweEFMC2gWMN7%2Bt4flTcQThWWNQqv%2BYhIwJjVGK6O3fLaTTMKa8UoUbfIDI3d91H2Y%2FdkOoK0MSS%2FDYvzxIZnFOfGiL2%2FHQNLTmzNKgaPNT1bcQQoQ7Fjvnf3VUPW4t3Lm%2FMEygh9AwtN3IPooKtZM%2BOjSD%2Byy%2B4AgmlQYHhyBuZ5C%2FAwpCBa7%2BDmqAjaJX97W%2Bkz8XQj0ZP%2BUeV%2FcVE9dWuvbh%2B8nObhiN7v0nzY84VfCJFrPwS8lu%2BulkuO2rJHzOasX7M4cs1nyqVa9PNNw0UTqWAOqE4i%2Bi%2BNDyRF%2BLvFbIO2z%2BzaG3bNNKHztVmPL%2F0ZxUqpx8Y91RV4A8HozXgGV17MRPtyq%2B2pzTcdKS21WPzpk3gy5YwAGnOTmMboSSf7DuTS3rySSD73n0kQtF7JHv13LqXrhKJtB87DF%2BbjM01EzhHDC%2FoILHBjqkAcO7p76TbXGfn1qWKjlK0k%2BiXLDUwzSQcOtLLytxlvCIjjrW5vOKJCQaBZhErM22ksYY6rp8ppZCu7iy2X5i8IH3FF6Cc3QjKwjKT7letqiljjIgecQzOfRatJq7jvsUBWVnIilyK5jKRd1Tr1b8y26LjwCJkTEL0Z9E2d9JSTEGrHbpluHDYuuZimaNUgx8PJPJGfJZmkPqo9ncFOo%2BgYTKdZ5F&X-Amz-Signature=3bc920376f40a26d701f55e137fd89aba3041fb7cfa7cac08dc3c5d83da70bbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
