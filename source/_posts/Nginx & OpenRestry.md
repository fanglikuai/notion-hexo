---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3YA7BDQ%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJIMEYCIQDPK9BC1e5Oh4LjRiIUZNHDv6HpAnw3yQleor5%2BCvj3JQIhAIo1WreiJbmHOvs4o%2FoKHd7TUFzPr21%2B2RoUn7N2TtpHKogECMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzeZODFcmMi61jfJeoq3APasbEqmP8GmUkWF%2F%2BxkJt6SvWStWp2YzSHXH596iIJIrTBjVjnVFKGzK2xiiHjjGGb8%2BLbAgDxZAWqFxIqWtDXRMiV0LfdbcqZcH2u4j2v4SRvvWXzKuu3P25ar%2F736xd3bmpJs%2F1xz9jbPhgJdbmP8F%2FqL9xjtFHR79JB51Vv3MQGVcVVEetiywTQVPmB1WIEwUbynzygK8K6ITuBV4X5ddNAIZpi2e7VJAbuL4irghd5DNxklf5DTdo3TVr52wW988fqM539uiUcAbmAN1DbKDnl%2F%2F76PYzaoQ7TrhbzWX5d%2BSRiFMzJ6yi8WPf9x5B2Z1lTVlCOINhXj51WX1PKud4zhzcT5%2BhkkiH9FkagCBNDbNuflU7ZmWmIrF8FVf2ED3FaXMSzOR5jnSueoe333GYMQVdTFwjRp%2FvwmAbuFSl%2FpAsD%2F9X8Dbzy141ROufJOvC9m0cERJJokCS%2BiuyfandIiZAK5aIEcEi3qVcRLfgpqGOi4IJL5cBKRYDXHtE6Yf7hnQuuAWQTr5wLFpJCRp4gVt6KtqiMrJA7SIuouRhNKQ5QB83pXEdRxtcNH2ti8VWKEjTNvxDJlte07K%2Bb%2FEJVK0izhaJURcE%2BcfVsZoYApfv8seOKmrUzOTDOnJrHBjqkAad37dy894Ixpc4zihwkT5V6owvDreaGkeHI3RCzW%2Bnphd%2FtIPyCyQHpOsGr0Pa0eFB1niIQdYLtk4r9XLqakqCLRILgjXa28gAyheQ35yrbLoWBU1yFcT1%2FWPNDKDYNgQR0zlBNlqeC8z5qRXoIU54kyyFhJGLGUFcS1nKMzQR9tgGludafwN%2BuPC2SnTbfesSvNVpu0UEX6xN5XIt3hiTpbz15&X-Amz-Signature=698e7b351f95646b80fbdb341297d250b2536274946a0c7dd496f26af8700a4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
