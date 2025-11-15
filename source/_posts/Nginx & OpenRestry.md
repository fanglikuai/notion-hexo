---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDUTSPSW%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUpEbhYF0ccEWcZFrnbK1LjbGB0yr6%2B%2BzCjj0f16t5OgIhALxpDCUEhNhm%2F7I3xFp4xX7YD8WW3dXK5yzPyT7q22rjKv8DCHgQABoMNjM3NDIzMTgzODA1IgzrxdfO6QoLOCl1IRsq3AMdwVWVSw0QWa%2F8UVENul3M%2Bsl03EcME3o8jMaGhT0%2BWspfsaz1AJJilIOleRDbz3tHGxu2fbXDCt5yvSn6yGbGExZdsudQEpGfg2bH2FDlbDccyY3xM%2BGK1QnSeJjXqF449tLXazrY8vVFtoROdKaOU7oY%2BePOA1mb9lc1huauesu7I1l%2BN%2FKldqiUfdyWnUDwEbVEhJgDkoO15F5Ixb7VONXSTWssYKeBpIVsOYdN1fCqOZZFGerJeNEwnjNoFg2%2Fk%2Ba9D1S1PQUok7qwcvwwZv3ITx9p5SKnLju8a3xOXSnOI6d0YHgXzyOEEEQwqoudQyR%2F4hove42ZBuECqWSVg3P63HYvg5m93aNhL9OCK8WBiENOHYMnVh5ymahwKB0a8c1qwZggtSoO63HHLNYL1BF%2BGueHpjqOYySJTYsBcBikuVuACyhq1XJ6Uvy9LYe4VXTKRArsWP0vOyw3gDDeVZZbShEflqitqxnpP6IfpM1dokL%2FPyNrZRewJ1V00FGGhq3HFMV3FpOlDjdxj2gmSix43W3W13%2F2YxYaoQWKUCXviQzMpQZzeUlpZ5li5G14attCi15RT7NLssuHWY61dHWJkIMa9ld8NcFPdrKS3%2FQ7qXpPpzBklmujojD9vuDIBjqkAT%2FhFdAV7eFSmW9ev8cpjONK4CthvGJEM6Sicz2N5TVtLtzx3qKt1WUrFFDLeYSomod5EmgkxrO1u5ADKh%2BWEyXnslBaGLulffIWEp7%2FAiB74Q1APwkSkpcPfqghMfh2SmluOV4JzJBGpTbKMXxhq7zHVJRuB2BDrkPEIBaodLk83KYgP8otwj85%2F5Rrh79Pjwmp2f8xfkpUbZSK5lPxruakH23r&X-Amz-Signature=1de0f1e60fb1738cc8086f870e66409257c91f8c6cf89edd85509c610858ec76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
