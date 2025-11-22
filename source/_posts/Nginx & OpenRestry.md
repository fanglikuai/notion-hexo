---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663YPGFG2G%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQCdWHjDKjXifgtt8AekZeyaQWDjKvW2TPN2LcqH12rDQQIhAI2wSTjWOgpvmFuJvI2OCmGXpeHxHMwNJgeA1elmjttgKv8DCCQQABoMNjM3NDIzMTgzODA1IgxUln808d%2B6eQXOrvUq3APbYOmgbDRTo88fV2u8qcG9JbPTmv4VTtdkaHoiP6t8b4genx%2BnZdjkzJj3D51vctmJB7%2BfVKSqaRrQMK5qSIUacGlLhb4%2BoQFV2Z9EuGDA1YlVeQZBAebggDJHNbAbIpby2kmCtqWCfM6KS7tlPgTJGCliLz%2BEcLgbiRTARQjpIO2wpNqz%2B7XaZESTCpUCN%2B18zWropFaTDa5MRfhjlovj6MFbVZDpIlOsFfgZtUkdi6OMr%2FPUZz1h9Ctdvrhqlcm6RwmBYe5U7%2FTFoaP1lBfFPaBAKHbFCjCtVn5KlutOxu0X1xvCXIt%2B8fbn9%2BshyF0TRJuQ1J6%2Fhlybvl64XUUWqeWGIlVfLvUmc7T9p9DJ9hD5JpUBEU7ca%2FMaXJSmYueQ%2BgehGY3YQcJ851xGHRYsohMEtgAgLPxI8Y97lfM6vmSoi%2BwDTRHOTTUwWhvVmj%2BrfHOdUAKtM0lUiuQqs%2BxKkgiUSE7tIPYj6QRMOrjvDEhBpwUaEnJJhSosDGcb%2BMFU17R8FDV2kKFPXIOtZSXxDxzN8w%2Bb8VbTk53%2Fjl57dBG5oMR6bQN9037xttVjD%2BiZtrK9VwriiQ1mRnlfKS%2Bmn2rWXjF8JgjxcCiDNxif6rd0oRFRfdRB62cAwzC2o4bJBjqkAYreBpDWl7OYdkldiRSBbvv2X5QJaP7l7m7lYfIyDzbWB6UrXxi8sl0Z3c1TqN4%2Bhij1a%2FPuipFlyMktNUt7RYFsxNRpLaUml7TkdDejsdV6tNn1keKK2X6%2FaSRG5ccormXBIbhk2Q45YCj%2BxVVQHzWmmvaDuLYGpeabLa7DaiMlRXxUX8HeZE%2FqawBFc%2BmxDd%2F4fdHJiVQkU8XTbANqk0i9g8lU&X-Amz-Signature=21bf16c02425c95033a35a27027f77f56c1712a47ba43f3271e9ac653b07b8ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
