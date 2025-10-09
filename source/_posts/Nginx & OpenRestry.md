---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PTRUCWX%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T070109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCIM6D8dhI1upCI6%2FZu%2BNikzUbfUUjMalwOESJBhJdt2QIhAKkLoxbYBMzwssgzuLVFGrYapZ6%2FmYtJFrqQYkPH%2BVCUKogECM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRKDcsVS46bozEd%2F0q3AN9DJtQgUobf%2BiM1%2BY1o1P05Hrj5DdghkIHCYGtOknAOciKdC0KneY5S9%2FH1u9LLo8YZJ74JH3xPIQ99g9Jgra%2FFrVVYYht76ncMj7w%2BCS6nSDcbyHjHnFzIjbkQ0%2FdFXPWwxpktqiC6lvUNbRKNjUVnsaYbzqBo3meHDz6J8e9AfALuoFm%2FWp%2Bu32j7yLXsQICz7H3EBMtw7PR2M%2FbxKbURAOcWp4pDVUGUEyv9b3vhsMaJnc60zR6dAs0biwnbMXIBP3VW33a3m16UCWmE4nsQugh0580X94Ay3kKX3d%2FPqi0RBt23WnvlDe%2F76fTgox%2FoZp0QHXfaR4dZYBoaf4N%2F6QYaOohSwJP6p%2By5u%2BTN%2Bpe7CoAqFNTfjvqdvQOQzGDcu8qZeblfUwFC4tt2RR4uEwjAZ4Sb%2B%2F5%2BNnWcJDFtOIb1vmIj5gUj%2BAgqY%2FZ4dECBY%2FzJehk5vEpObcA5drP8X%2B34xHgUjNtwmxP6JkR%2BmIKZ5DoX1XWKUPe6bgJWEZw3fnNRX7GCEFHPCAt9MZAJPcV%2FEgJ3KOCj9Ni7VQmaNfQKwKk0TlARyjomqvGEBUYHc%2FgythWsx66s6kn4YDrPPTRxALOpvQz1wgIRvqvDjzXyypYKlt9eD2EvjC2nJ3HBjqkASiorV73D%2B%2BTfk45lPUToTZwPYMvgWVdihXxfRb5FaY7m0FkHdjPOhALQqC5%2FvOJvGMppnGsgiV42m0wVHRB%2B%2FOvNkVXUSO96xL53ilkpkc9Q0mYLDt8ABbBsKKUx6R8jcIgXMONJNR9KZMRG3mmPeqMNzPCzE1iuo7qIbWZr%2B0ieThvlLw4DRUpDPo5jQ84VxG59%2FOxXfI0XyTjJBvBmRtSThnb&X-Amz-Signature=2fd0a05e568f7198e26e996db168481a661252bc6a80317b99aeb30f39e08146&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
