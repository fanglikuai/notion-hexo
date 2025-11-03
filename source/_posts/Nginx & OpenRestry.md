---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5I6ZIXD%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDkADk0KKqvs2wnToCi%2FKm%2FRpSJfkXpgdZn8xsQunOzpQIhAKaQ0JWncJiu%2BAliPLcxMK1CMkM%2BahtFwlIfsOK7O%2F9AKv8DCGUQABoMNjM3NDIzMTgzODA1IgyZUHXeSgt1UgCmigkq3AOgOKuN5%2BhFJ0GXJISXQYfYnlvKGV0WIUIAlVpR8Df7Ld5dn0U%2BfuHkv8G%2BhE4QMjF72OPx2Yl6Xr4RJP7FaiIz0cIrYtcWZfYyUWfK7RX7gs%2FWPrvwmghm%2Fry8qVAf1b8xcJ7xR9h%2FT8E3u2n5aUUy5c0raKjU0J8CGckUPsNmnQAJjN7b%2BTTfoQAb%2BGREXE5iCxQlDI01NfVUP2UkAXFvSLMZVreyUKCSzrwODDULOS8mMNnyvJNYp6VOcHson2K2TCVfWvfluCAeNhclW50nyn0xvKM7gGIh0trBQXc%2F4N3CET9R4vh6w6VGB5QXpxn5EdRgcvI3r9AkZf6w0RSfHk8DtaNPTLihAFqQ2xZhtOHg%2BLinrwHPNkK760KRNynz00YqhridXbTBAvZq%2FqqEPV90gG4Q5tAY%2Bvt%2Br8qlr7K9RKs%2B0xNAWuD%2BDp3EFwFHCvmp8aGd59NRSVGxoHj27RJYZ3uzPYTHuBPXfijGpy7p28o2%2BiITeOI%2BtQofcganiV3AMZ9c7uYKh8GlCEcVKG7CRkbP5sYFG6jccGyu%2B5V8x4xWeif6prtse%2BoSAOuJ6NjOvPKBZZIrZOXE4BdlXojAvEYIWksz07kt9R969e0cxkxqxNb%2BckpdIDCEk6TIBjqkAcKGOMv7rjMrewlFmA2rqzSi7zqNSavZ92OmPJ%2FWKN%2BwFOqzdPGVKlgAlCU1Pta55gyQGPsCyVajc%2FK07ihUyPrbr0jjzEz9LfQ9Yxu5TNSx1I2iJz8avoltb8esgSVTCBRlFDcp2n2j%2Fc%2BKrDqLwXzMs%2F0mJeZqlUSA9cpYTKtKbswaV6hsPVnsTvpg1H2bupPT2OEHM8JHfEX7oAAI8oxxf4q2&X-Amz-Signature=4aecc3d483960c42f4da9ee27c81a1a18c688261b6c649d9ffe3fc8aafe00fa6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
