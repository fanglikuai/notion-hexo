---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7SDWVPG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBDWhTGP6Ta9BzPHM6os%2B68hIlicrUzdejbspVEklYFQIhAPFDPORkuv4X4tmb8Os3uaJREtPAzG270JJlyx18bt2vKv8DCGQQABoMNjM3NDIzMTgzODA1Igz3mPa2%2F63lflnBFgMq3ANcK7peP0YphybJZg66uV3fqI6FpuKoaZ9ZQo6dxuyS9oYa4mSe%2FgZz04bwU%2BOECztQUbjCf6OWroyBZBxO30EUYSQFoyM22mucD4sVxmzH7SRzsngI3kZZ0PbTOnfMbTYctuJa6vazP6ccnDvUVeElj%2F74XLTjiZ1QIiuWo7pXj1mEaOokY8b2yss0iSpwAST7QO%2B37Wp%2F%2BN6%2FEYUxPbbgjK8BENnEU3utr2wjGFDpE5kec50VtFPvC82PbAYh8b3XAm9Na2oV%2F0bw9tGq49uxI%2F8yrk15f%2B%2B7bgmlExwINIbUgnzgxJU7zYlTGAiObhLtFBQlfI8n2RDGAAIAmKbDM1Qfhz03kAqMSLouY0aUwBLeXQd8ZqmmF5cRp72zQFPVpUqMwraEfQqbhr97sexqZocVFS%2Fu3d6YLkY%2Fkq0zZLAiVeG6bKHeeekuxTylu4L8ovSIp%2BWosbFemi36g4hHEPUJUp8kT7crsvvPpha1NuDwErRh7IXjTwyfpH9SPU1KWLVRHEtsYs5XCF9jITZBpJHHBwkm4wjswGqbhDKuIUihpUEgnlAa7LQQApzhuaCsWdymlebOizIm7lqkyVNp3o%2BpEYEW5rGI%2Bw%2B5kKYPpCjaLgDildmuC4veVzCEke%2FHBjqkAUOLhLxeRn7t2GSbMvPd7%2FlZYyTTzsLdu73CUoLSDAUvnlvuIhXXp9nR4vFijow3YUJ%2F8iVHONopln44kqF4J3a1FYkfmZBgRR375%2FK2hOaYxy3uYbqcZsAMlrf%2FsrSppRO4ewY2KNhHep6OmqYhduNkU12tOeNto95bRL0ANM7Ppq%2FL0s8fTX4yfudA0KpdAdnyqOwfCpHw3GpdmSX1B6mRpT%2Bi&X-Amz-Signature=5ab94b9200998483d18e978f24616df64b0aa18857f4d76255801b37dbf52911&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
