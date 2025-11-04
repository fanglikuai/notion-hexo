---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKGVHRMF%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpKCgVMO%2BLHLKk%2FCJDkysw%2B5CWcrLhLYqSkuDdFsB2FQIhANjo3QfxONvowA6GrbcAVGeIrFd3vOmgKGyyC%2BHhAqzuKv8DCHUQABoMNjM3NDIzMTgzODA1IgzkWjdGuY%2FJOZCocIAq3AN3ZFk%2BnW%2FgrGhhnqaccnvfSlr0HEEQVanY48SL2iujQDgcapYo0baeQc%2BVI2fksKr7ZviSF6jWpcKYFvrH7vtpP7XssIBTXf9yKDlYSxnG%2B1GbtE2SAj2rjouwvnO7A0cHfO0S0ue3qeQ11ZbrRDBN7K5vZEWPYQ8rSZNKISoW6jSJ3fUX3wij5T%2BWfPVYIq0hEWleY8aZBmBbgzdOdf56DvTWTTfzKT7dJnd1XtfGSI2AquMRX1T%2FaIkYOdzP4a2SDX73nN992LFq%2Fm6KzTMhFY5eA5R8LguaPVjgXD7LWaCskbTqxmf4Jd%2BPiSsDW7dqF8wZN7llhlazIyio8K5iMV9Tw8l%2BDHx1dqQovkPyQ0m7gkHmfE0rdceOGzYzuWWDFQfX2MZ2F2lqFUHEokhFuXnWy1we7OEWaD4mn8U8tIwGQ8HkjfnJikYH84eedFEN1bktLx9Cm98bEaEgvUFboRp7TEnjfFe9K78DlLs%2BPJ6QBXHNrc6jv26%2FtaJhnvV5UPP8Z5gBCO4wt%2FsMaqNRfYOCY6quOMYNK7mT%2BIJH4nrYUPuK11ayl3w3pW420CK1Dp78RVT6yupja0mYrWv8bEbgBTi73wfzA3CTJ8Cq2UFU3n2JubLAvgpG6zCe0afIBjqkARaZAvUK3JZN4Ldfa%2BsdwFVUVRlgFTswn0rfUGs0DvLWl5IpRL7I2I9FGfDtol8l6dQaKF3UZk4JIYM%2F7v1ckKGtolLo9Sp2QsvkiKXuzoleye8iHqmpLMWNUxRIqeO1iFgvOInk68Uc731%2BSdtKTEE32DFS4KOkspZOmxXsfFeQIgYEyN%2F9Xe2z1RSVP1Gn1s8gUGp1nL%2FdBV8jNMy1AY4Hsg2R&X-Amz-Signature=2052e27783cfe4667a3acb96d7e7ed535e8c2c293e9e4472ffbf2ebf6ed29131&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
