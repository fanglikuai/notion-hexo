---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYUSO3BX%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC2x2xO%2BmKeemTH61HUjfCxE3KHJeUysfDC%2BlWhzTme9wIgd0a%2FFqfy91QXqbCJJt5rnj8EFhAflxstSTmG2G7D4j0q%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDIndyxgXJy82ZBVm9CrcAx%2BdSrJTb%2BFSOeyoz98sXph%2BzrAWRdW2DLvglJUESuDWG0I2ped56ZNeL9GjyBsIc2lmT84JimKHpRBco5ZiZxYyt5hH34ryGDoIsJO9IqsEorHebhY%2F4PWROixrtfkledu4z%2Bj850uEDC3D0PFbVuCZAzUns4S3s6Ka7qWiGC83JkXNNT8ZR%2B9XbK8sm7yWlNMz0TDRFj7UDDZyrTvKKOuee6r7FARo1%2F5gNO5x7Vwx%2Fe5lpCzBWZr%2FqkMoxyn6CvZkv1BTsFXhin2ij9I3janKf%2FrdsNcWOvQYeY%2BM2dbkx5H11%2BMzHBUZ0YFzZAuZYnmaNVJKR4u5ZOtD8rHahxgfNEDEB5Lid9dKbeic8cUAD48i%2FMAVABhZHTuKsrsxg1LKoM%2Ft0xMIibdKB7Ca5h15fFME%2FXEgD6Lx%2Bg9t4jndxWJ5k9UfO2Oa9AOACvBiDpz%2FxXUb7NOczy0zK9xLZ9Z9NXEd%2BL4mzaIjTQclkIEsCFHUH5a9OxgeEOQRvc4KYpulheLXjygoUsSheTFFfnIBwTtoA%2BBiBcAm0e04lMvEMkwR219dyZ3meEL6FqnHH%2FXn8EHbMmLjUHE7E1B2uK3oZ5ofPy6SaGkVTPVtnlTPgPidIcOEsWTe8qEjMKCfpcgGOqUBFZXOmWlGlvXOU%2FChns6fxvSqNecXJ6czTJQdcYq5jYvOJ%2B8F2rTydAxZ3LMxL7z1rU9fhNC7KcoGH6Zq3dGVCZBbuupTRp%2BSuD5U6K35ArLkV8ozhKlRr%2FCQ9pzcBvMLPhtkbr%2BA64ivugXTjGtpjCaHlHdTTsktBr8ao5XvHhYwuXNPJG10nmP52nMfgaBVspS6La1%2BV6OxHycRtxwX4S6wyFg5&X-Amz-Signature=1a7fc5d7aa9479682c8d966bd9203bd6287c54f4da0683dbf8bb89fa808c2654&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
