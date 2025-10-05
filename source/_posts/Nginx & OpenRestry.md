---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMR6FOCF%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHIx8VstJkzaerLDckYXD99nlumRdtFkZeQlElFMeIm5AiANJKd%2FBoAyoV3y3bE0Zn3paPXoBqEhACiEKYnmLWP36Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMCq0w488CLdbQn68HKtwDSSEbbV9smNkhkDmrIMwgtudmZ3hESm9H7CIbOAQzI9NVuEqXOVBEGlnPi6MgK3JugiqGt4yrKFlP5uUn%2BITCr9lNNYKlLHURqCpHo8%2BrXU%2Bzi7FwH4qyL8%2B3A9K2ydfD3xfppdU2c7K9%2Fv45%2FlMt3bZ3M3DhLFohCRHSsm9Yvca4gn%2BQk61e2byybNADZ61AIt%2BEdY0yUsgUQUFb8XYq%2Fm%2F9GwGkOjwN4BCgRq3j6H7bd29QKrbrE9jyM%2BY9yaPyhK6qlAfvuRyStsfvdLBObvUQDYEYp1%2FS3iAQnuX4%2FZcxTje95bdCcbi%2BD0L7pdGHYsGcovWBXXRl80rkmRUhZrGS9IcRKfvEk5PzOu1ffSFUBsdN%2BKtiCD0kASfR8lCpR%2FGOqK0k4III89Sfp4csL139Fdl82O%2F7Hyh0FnfERzDw3BPCtmsgKGXr7drkucueXQgKUrYaFZrpEokrLSEPczpa4VmDQNC9pmdr%2FF2zcG67cgwzZTDa4t%2B5OKLVlM1bpPtGZj0PbpmwU8YI0tePm5AqdXpzOxKAnHwc%2FSGfIQPSvq4wywjrrMRktZ%2FrLa6Q5zCA3QDQwJG2bSKlZuI19DaDw1GKuOKubNLY7%2FwDSCPNvcO7n8FZX%2FxpPjEwqYSIxwY6pgGlplkixAVc4UUTiMXlpfusQehhWEekMOjWNzbQXuUFd0Km2aNirCTXtzWCE4zyYo4o3FHnlm4ROr1H%2BokPlwkt1%2BV2w8qJQcb8qh%2BJlQGQnpSQYjeiXlfZN6HoFPoy4ebI7%2Fyq0ssN92yr9H13irUpuUfCT%2BLNgncn%2Ba%2BSjk2U6rag5wm40t5pzLMKbiOufZQdettrmgQgsIq6WP%2FyBGefDxEE0XA4&X-Amz-Signature=57bfc97a4c9f400793dd95d4055f637604413969c8e61414026a811fb5a1fbd4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
