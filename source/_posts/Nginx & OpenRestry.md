---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPNAVZHZ%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T190052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQD%2FBbZLMXTJjbywmXOHG3r3ZG%2BQaa50d44IN0xJj7AcWAIhAIe91UAzzMra%2BxdjhDxZLdr3vu2RFwVIW72RaDGR5E6WKv8DCBsQABoMNjM3NDIzMTgzODA1Igx8zmUBwkkTRtGgILEq3APHesxTx%2FHRtSrx9HQmGEsRFDhdtik5aaDSp8wc8I5Ear6rNWO3HEQBobdmW4QCGOjBiki%2BYsJdYQYS0favB8A6yBOg4NAnyu8%2BPo3IJfuL5s%2FvEuZzZGcqB6TKI%2BkT%2B3pVLJTMQwt8Ja6geoCIO2eMSvxQyb4jp7VHb7N%2FJqB%2Fa7ES6t%2F%2FSh5Kbf%2BO7FZYT6fJbI49egSOfBp5hG14YqJuyv5XIDxuZVYuZspyqJ1JUPLEUQUB60AQNHg0GzokLbD7ZZ0UJOIwL7ZP08KIQEYocPB%2Fm5gj3cpAi7eC5gMesJJYy7%2FUmOX3mGOl8HJUVBDGkStc4T9eLFvPrADBlt%2FALoXgRDMZQ1YQWYuuZd1t%2B81WqLFpJkv%2BNio%2BogDjFkBZvq9vKPF8bb6PPaUMV8s2BUzN5JequpseITj55RLcl1g%2F6oQjuDuc3zOLuee72kwisG0jzr6SrBBSqfi3KhqEfm2cvtUsWidvj6cDDgLeURCGfeCRwg0kWv4NGf%2FG2mjV1Z3WAumc4eZ%2B03dT9sqzuFJcpufVGNITLsbVHYUTsNnSWkYEQvnykdBBthygUbstioJNUvTA4wZ2uGoF7iPRFP7n3S4WGUXxA1a7fsp%2FIyWBlsxJQmqVIShb2zDlk9%2FHBjqkAaIKRNJPDChlQfGOFZcLsy5bi4H%2FtGfKFDODF1jlI6yL4IJEYshivXIfolmogooEpvKgU5%2BZ2jxuwElfzAO%2FiYyeYMNThiT1r2IVOm8TCBYO1iuqooKUscsiDbSUdJkEcBzrLzMM7%2FY6mMyAw%2B1tWTWuTfmoFGeD6ZcuHjkZMgg%2Fg2HOMYcyCGLCjnEisOvIM1ZfK9xJTvxoe%2FNns0QgvZVN0PJB&X-Amz-Signature=7c4b05812ef9fbf50a41f1fa4e770f9189ba8b822b30e25485f41ece2a2de256&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
