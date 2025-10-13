---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDYBDT2B%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiAEKH%2BisnxWTur426eOIZZVLr6U0B7bnwgrNApjg50wIhAIcFj649Zkxb5soXv35lAtaSlqu%2FN7c1ySMWUyQfsOkpKv8DCDwQABoMNjM3NDIzMTgzODA1IgwpKqV4XNhyKNv9NiUq3APPCNgG3YeFvEkqoWA%2FuNPlox%2Fcslrv6Q0QYPBQHyXbNpkRbNYudwZJ6%2F99Xc5NQmLxYzINJlThVH8l59qlmpJqbhCcrlOHAdDa0ZoNOu4q0hrXRfuhEj9l%2FMvaeMTgG6a0RjlyaOwHf7M76jHV6ZunqLMDt0249zDCgsznC0NHlE4QsbuEutDHo1NmdokOtBqJtUbMD7nhnWWNdzUUx%2F3joVikPMMVgebfsS5tc8CV1aPR%2F94SNPeL0Dfpuh5Fn9mwFuXcO71psK3r7GAoII1xlMz2Huw0YAmFZhQOMgxQyxCRsfEeRakh4SVhGd57tzDRHRjX7JTij3%2FUK1wsytZYGRzX3LkU2ywJpyXHvQ5ndAYddN7oqCHcKRPORn6%2F1RiUMvMzTbIl3zrJGO1ThdlTHoau4RCUxkvL5oy4t2kHpXNA7IaguMTLjR2Cehb8HYf%2BREm2gdZvmGzIW9RJEE3dRbu3AIwrYyroRZmuDisBxoPhn8JcO7W4oTR85ilqopdMVeAUgIgtHXd8dSRks%2BMua%2BJnkroC1KjXQRi3mu%2F%2FTkc4i7hqveWESf0%2FuIF4ck%2FZePy0GEpCyoZlYLqSQTGfK9QLWbcPd0%2BKNFJGB%2BwT45L6xUyXHabHwMZ%2B7zCQ07HHBjqkAe%2FR8RqbtOAd9P15%2BDMcKf4iMv1u8JHZ8CGS9uqGpBmceixLrz8g2DBlj9XLvKBQnWqP0RsmQp0StFlq33770CFkwSUFDGNgoqDXnprQg6pSxxosMY%2BPWcc9vGc43ZQLdp03VnTRYPgIGeXh82JOWp5RebpWLrl1WzvfaATMiLmXzqUcftrnZhli386IxeMqlUojbzBSkhqrXjKTrn6b1hioJ9xw&X-Amz-Signature=2816c7e830f8742cc7b85e94196b4497476e1e5b8939e495297fd54a29ea17f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
