---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624I733DT%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJGMEQCIG1WjY1ggjmWxjVYbvpxYhorg5FAY0A82KOCHxb2Q%2B7GAiBfwHwB5u%2FpHmU4ybFqpkjCoeKjYrQKqq92xGUlzg1PgiqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZWHlf7lD2wwbmVHHKtwD5LPIO6CpA2jyxbOvmwIJR4%2BVtik0k2iP3cDrBO9YCmHutBRirDuVl3%2B22lnYTeXDSVERW2y%2FATCsvqY3S7U8JOZztz23IlQeoWpcCbmrNCnokqvUxbmpL%2Fkw3j9VydyXrPwB6U6KmbpMtZwTPB7tz%2BkIeFrCHQtbOk3Mn23%2FVou756ArY36wzoxfY4NtYHHXzY77dbUepD0GvF%2FyeqmIvf8ILR6AX85vhVeaveIONs4AZom6y7dFGmbSqZxrOsANLKnZrpf2Knshn9xG6v6NqJskyOcbAJmqSJ9L45gO8EBpLehpZnkx%2FBDhQi%2B8xGwPErcyDN4atM1VdJgO2AZQH2oO6sEa06nf80zOoghNjzmCdMEVhHqIuss4Bc2p3UK7UhwsJPRbR3mx%2FZ68kpemPwkORopEnBfz%2B03L0izrvHYnMSg2%2BvAyrq2tLvCurWDXzVCYZM6eqLUSBmchuKa56RdOPEyZmOI0vB7zqqkcoVx2E3LDzSBxuf02oVd5M6YfbY0BY7Gud%2BjUBpkF04Fo8E%2FPKiT6JzEJpvAALMQl2GuBskTWCqwbV%2Bn4K6odwF5qVeBzPI1zAO0bS6v%2BM69P1reaX8b4N7axWVEAX0hxGTupdGkszJYs4w%2FXZhMw58iQyAY6pgHI6JyEbUW5Hw0gq9nTSwgg8ajWhLlTtzbgZajKZxXSOXXeJ83nya46mmU%2FkEOZ%2BxnpeA%2FRqQeVYeIbCyI6mL978XNmHAP0J4ZBe8ZGvoGZp1k2oTi2H6JYXY%2Fegwt7LCCMcQSkdS4w41u1c4Mxy2GdjOLJSOtBaPc86TzSYLQH7DgnieNJQtY3C1NU69vILp2hJMmAH43RnluGPxVFopzW1b9%2FwB9D&X-Amz-Signature=ba106d7380d6c5e66ec75aeb6b6eaacb5165674c908a845f36a7411d61ebf160&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
