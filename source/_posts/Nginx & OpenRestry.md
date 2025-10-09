---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QUGMITRF%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJIMEYCIQDWmmMV0PkJKeRaPx7A1ZF0wMZrGLhlz5VicgOLlcQ%2BygIhAKCERDkCZDz3q841bIHDXjB8Em7%2BUJlM%2Bnr5oK6HzoJ0KogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzkxn9KYrzC7V7Mqu8q3APMC8z%2Fhd5mNt6gQHrzvyaI7gSpDPusvemoQZ7DoODE5GaXdhYZfUIctoFRrC1PhGmQe3NUbT%2F%2BxNZpdsbwFhWgjRStwWapsPoHZP7j2QhdmUdwB6utOJT0Al%2BJe8EHhF20rEI0s2HDHpbds1A2mOJ46Q%2F8LvotNY4DtZOJQGDx61jeto6jMUCFNYLvMHvgWx2JWUSKU%2F6MbbyJg67q6cE284etCAMT1p6exOiqX2EuHFhgei1o0i7JZFGe0tjqaUgkcDRuDjv%2BMzvgDeQfeM1qx7xGiAP8KGxeMr%2FbG5itbqczoinPJufXpPxqMdXm%2FtFBHq73AmTNQbzfCTIRcYcZ%2FndbdHbqAJzRF60TI6T1cmQYusWnEvdz2KnD%2BFi94%2F5PIA%2B8zoKo%2BGb6GQvs5RweRSPZ1l9laz%2Fgv4OjdaA4PQR2UDQvui%2B6pNdFhnBlmAG1YAcnoYC0jMYl70XY2wN5Yj2Lhp7rcNq2EzsW0lxeCrjHn9BR2xQToDmZxDaLb7v6OB%2BwYhDsOr4kO2UB%2FfolxJq6wNJcB16jqhZn363YbKXR%2FvwHm5XdliOYo1ox6K2ulI7MwyitmDZ%2B%2BCcLDXLUXmRC%2FfFGab7FD9HzpfqzmDLGVRmfARyh26hutTDhg6DHBjqkASxNHpFhnqbF1Fg7z1siP0cPmMlXpBB%2FGPsH3Bi9Qn8tvxtgCboibfJ0Fu3kg97tEhM51dEp4Yncf6Ufbrt1S4sGSG%2Fr0meYQFmrGxxKJzsEKqzzEGXRzW2eqxpBWKecKEkAUX9AdBvg%2FgnujT%2Bp2mj5flHDFl5w4UaOjEY9FEjrWCyE6s9AyVHVH%2BHd6yiaQBzP2sYaxWbt4JevIITStq2eAK%2FT&X-Amz-Signature=2368936cb4a7f1471334f3875eb70165205f6bb37c4f8159ac9b9ecf46b96514&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
