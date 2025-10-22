---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XDUPIJBR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJIMEYCIQDDo5JL7kzfVxAPSsAJZvAlRwUoJ0u1kSnlQqSlrcT1zgIhAIkGLXgVtnXyRzeLekmWvQ5TZef%2BFM0j0be6jfm2yBN0Kv8DCCwQABoMNjM3NDIzMTgzODA1IgxUAr2a52LV39fsJfcq3ANi4CHQBm2CGQ8IxyiWeh%2FeJE0WVTMei5quqSWmM30B52Cg2UqMDdE5GngEi7oE%2FIduiKGarBjdW0KCCASNIH%2BgUxQhB3bSpZ6bk%2FbP64dnkcxRCkhz2uYVTOBXNGEbquY1s0tl1yXMVTNMfAdgP9pr7idAxTcH%2FuEXpIsAc8eAwlJyUur70fidoyfIeofyiBdFz8iiHeApZoBCpqDx6fM4CjUdk4%2B7%2BQo71Cdl%2Br5a5wA5w447E7u2lkt04TiiihYwDkVdlGB%2FxahWcXOuHy9tt1JIxFOQuJjDXg9Zv1T7NcnNwWolBEDxlUtF%2Fg8eP6HBGvcDapeqJBcwgQu2jrAqh3xY1vbsRdFzyFky1Z4DB6GHRPk58V3o2rVXoia9NhVk2WSGcUDW8zBv0bAG2PUTe8wyRChHjSUf7PY0KZeu2giCf9UmxzN%2Bz0A2S5NFSyrfTJu9BTO6aucorDBMvxZiE1%2BFtmRMdydJ6JGjymAK3JsVVK2UVwjGEhbi9p34c%2BnwAdgBDSn472Tk4UGXdrZf2Aj8LZ%2FSE%2B%2FfoR6FPlAlNRui8uUHSAmKz9yD7RHRzGP7eHH7v%2BIAg4JQw81Rf%2FUg7%2FboarjHWevUCKSY40KcwzeRAZ9geWWyDvev1DDk%2FOLHBjqkAYCMCeTD7sf7eyoDEo%2FgsiF%2BYwrfWICGxVOgdpd5r3jKDG85UJ0LpFt8D0CUt332mO75qsPcwG%2FZVA%2FkivEWdUCVEjJ0tgG7FY1TbfWK8Z%2FCkDwuEIuhVFw5bNEJ53g1GR9kbEGHBr2Wtdk9k9PCzT%2FTwkkUGm4fUqX%2FJensMalzfrZdxvb8i5a4IJ04sjKX6iwDTifzi3rtiz58oMOZflyQJ%2FDQ&X-Amz-Signature=4a33070ad48ffa1efb8f56a991e2cbe7de458f80926bd067b51e3963a88dbf1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
