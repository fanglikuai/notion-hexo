---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XBP6OEK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIBxeUGsV07frmZ1XuY650xjMmoBBD92YSI9osoSc17W8AiAgCw4oQqvDkM4cvtCjlhwSmT6rSSvcn9ANLZeUSFvzLSqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZp69vFpMb6GvPpgsKtwDkmjybnuMrqv%2FD%2BDywEh4JP9Fa223K6hfYO26XxJfj7E5uNpO2MSc1KLcP8cYlkDtS573t%2BKRreGeHqWPrEqBAYEQbk88NCEFBXaIEFcc88%2Byp9m503HHLgg8KUIUj2cO%2F%2FPZg65AORYG12ZN6s4onKokkSPIP5NvNuuYJW%2FrMAC7gvRHN1ZADCWtmJWmwNhJJfLvrpufRH%2FKzx40Jc%2BKd7N6V8kzqtcO0vHPLbPU78DYKWoOwufV2zQ%2F1N4rcQVyJJYXjNKCAMK0yuB0vRP7vWcwe9TBe6a3uAGb5ufxbSsCaFir6ODSIkuew8a3eauaVYIYARob6uW167qBgl5z8MY8ZPi2p5O6vtTQjXKr7ZyHgFgqvoIAMIt9IP2h287mjmOupH04LHK%2BpaO9mB2j93cuNsp4y9uddXpZdYvW3m6WUnaBDVfQC3ca%2FHbUCj5itJRqopGPhJ%2Fg2UkSAewVWJexskCTM%2F4jLYBWglbrAHpuzVpT3dIPtabuMkIAH0bElSVLnJRZ1voEYqX5FnIHtFOKxG5pE9diyOTs3ExNyfqzT9yOHSBN0mWJlc%2BHrPGAapdApxLcaSaB79Eh%2BwjaSavplgMrvCuuw5BugxMgAiLfRORuEj7nu0ZSUi8wgq2rxgY6pgEy%2BWKc9gyGJ1uP6bg9MdFoF4aNAc%2BaEHCQKvmdcb77TNbBI4NSrO6TnF7w2tDI9Av4dSnxGmJu438f4%2FwHR9U24iSneqxoe8dNLaR%2F%2BQhAJqtRkDsBcBRkjjvZPBHVc2K2aICpzym5QsUjYxYov5USX6HV5Hb1ggfMkT5gAHAzNFNTBQCDggoXO5OzJw3S08C5GOfjH5hXP14jZHmP2GunkkSY%2BcMr&X-Amz-Signature=fc3c99cba43462cdc3a7b4c90afad3972ba1369244bedfad58ab009272e000b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
