---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKZ7B222%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA09FIxy6AfrpnEDDZyNhr44hor%2BP2GmlVMrbWrJ5PoIAiAI%2FBf8xcwO0lGCoRLlpTaLZDVz98w1%2Bf99OXyxiG%2FU%2Fir%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIM6eJE%2FLQPNiy%2F93C7KtwD3alpR9PteFMaKCXQClvvMKnu2oIIxWCsEnwyzFcoBgA1sckodkJ%2FSqrAVMZ%2Fqn%2BkJqBf7IcWYNEgtQwjS7FpHe5%2B3Ednt9tEgUFR3c%2B5dNzYdFX9onUTKNO4aWOs5NDotnJ64g1B2Y%2FwQ2Mbi4aP%2Blt1KZiIn195vioy5eYQUBNxqp0wiNb9IbkhCxskFw79FgVIZUZmioLwlse2S3H0acva2ZBsx%2BJKD6653HSrbHT%2BC%2FrsYa75L9V5tXv5JHc%2FvD0EW%2FmP3O3RuaohlmeIGZy%2BeJRFIDSxWsqulUnBYoRjh68q7YpBVhUWXaedUN%2FIcAG3KLCLi5ZhZX4u4TJtOu4cq4da7PFnhTpR4hyF1X1hkzEKIGe5iDhqotUz%2Bv9CrWdWPhlRefR8VilKSEJwS2k05Hnc%2FsG1%2BKlyGk8GDQ6LSdPJe2mT92jtwKpuvfe91Ji3D7GX4vD25XKLbthn1Jv7%2BZUv1guw3t0%2BPOgmQK8UPZgqwQL5HhsxZlsb4wHWFeGDMfZj1wYvmXvYPcH2lAcGuz6OpgoXLoDxKP0aFLvlJkuEgTFrKDgNRKdqmtPIneNiDU6i1i58gUXCpnJhXeFWZvVOC1YZRvEml3wDPAD2pNNWucxaTLN3EUQw17iuxwY6pgFAFqP7ZdWNWbKEUO8bvvwmY5%2FoKexnkr%2FCs7DY7YqX0NcjD3oSHTuKJpPsCD6ygjkPNE7%2BvgGuBkeMv4HV4PYglKTjFIcxNhyKpG8kdgS33%2FZmmrDXHxR2MajOmUaPnwfjEYb%2F4zM9TNxlJHn0OqFTf%2BHNqNImI26FkatNGCJQZG4JzwvPPV3dBG2u4zIkV1RN0jdwN9TAQKIcZ1DJhVqoeJ1MUZaw&X-Amz-Signature=e832d9336d8f359517f10845853ffdad3e41211d7680b2aea015065cf4ce0ef9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
