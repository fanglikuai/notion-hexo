---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VBF5HX4E%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJIMEYCIQCs3H0m7QevWf%2FbckKCImS90Xo3AoBjWTuyftAlODYm0AIhAJ7Kd2x39ZH6b4IPIlxsl8Wpp71L4jQckrl5tnf3VQJuKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzaUgilnfruUMjgnu4q3APK9FIAnO2C6th79vy4%2FHNwLu9UORNJ%2Bje1gyVdnCXTu3lig8xRqNLTrYi4PM0qBOE7kx%2FDcQQ%2FgbsrRJLaAKUfQaRH45Eam0WXvo6bMuF5pK3qBU%2BNeK6IHtqZx%2B8j%2B%2F09Xy8MApj4seVNRN5ovZMPrS9pLHJqeUMRpCf22u0s9wrEVq2f1nvdkuVCv5EpenF8u1LrE285Yv%2BcBSaO39%2B%2Ffmr2tJBD0P4nD7KZppe1pFE2neZJCIgvtxkqLCMrLBpWbsft5TpgqHJ9zv3xOfxAzTv7SiVf9femFcRYEkI0CBeBBybOc1P4fKYq%2FQF5wO3rUOD4wDeLSErnJV1ZwWYJ5P6ikgUnJxnM55RqAHBP5baOlNeHJP7sLiI21ievQb2SDQiOulrIpuAhy0gfRQc1Hed%2BJ7pa9MkgZX5sPYUk324Qel6vvrakAbuOfUvhGr9l69HZyj3SXj5T6%2BkCoMrvOf%2BrXtt5wBROnhCq%2FsS4ancO9DUw2bhqTZ8NE8oZoBmFbHah74xdzCnzlPbWajBbuVzlW%2BzRjfh5LdNTPmsxYOYdnIm4f5rNhhFwaICNEyNBmh7jXzW%2BXIJAnBZVaEmE3PZLyxz8qCkDLqT1fgmGI4I%2Fng9uV0xRs%2B0fvjClwLbGBjqkARa3wEbh4yg1xjGY2tzbnKAMdwooPuKeSh5nKofBO1Pn7vGpjfTTYBgINyclFOIUOLWUiaT0vqahJZ%2BjklAk1tb7FfecVsR2sjmZxXsBBZAYGCzdbZ3Pbvy3wsm%2B6t0Ha6WOBd1EejwjdDQb%2F6Z0KcbZTA8yP3rkUYLBV%2FoLE7TGzY2K8cncVwLa099b7UZ0sQLHbTON4f50bso5M6i462mbZ3u%2B&X-Amz-Signature=33e4e0af20cf5a4bdc4c0de2b161d62296fe8952d2a8f72616febe38119e52be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
