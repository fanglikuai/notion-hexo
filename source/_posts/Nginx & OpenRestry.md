---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFKE345C%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T120106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKQojJJR%2Fe9%2FUCJoukUItxPBhTP5iew19a0lH2B94Y8AiAZeWxdpKEAGQM0XRXqRgFZ6C0LUFOFVFA5WsoMKTwPqSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAf8m8tmG9BoT18euKtwDaDIX4rr8i2rRiTNk9QVTkU7ZSKuZmLe02oOPxcgOEeFmkosN3VhNg%2Fk4%2FpBJ1O3Xfi6ng7Wh023Q3zs3En%2FT3mbx4sOous6duYUykqdzNgYTEBjuRlTum2JZWajCyChZ8E%2F7mftsjf0%2BQuuGYGgUSneoMWKQQTsn3QNP8FvebPR1XDYh5%2BbTfzVWaBZBjNT41stb6BcRENnYrflKAvpwuMqbKOD9hKsQFYlR3eylLHiKIaZrBANyFS0Ng%2Byfo%2BvpuqzIk2tuo0YvXFjyeJ514R8ox63%2B7N0lsku7kh%2Bx2cclie7vJjLVxBBPnYRhK%2FoMkDRD5iuKLEBvN4VL7lMtBz5PDPF4uIBpBtuiZtN1QeIt2pBdsaKLDVRAoYNcAGOAjoxPAcKeOaHdsEafUTfxvn2taasulWlwMq9sZrxUvLrzw6ILLer6dkweyYS7GbT%2FPVn6x6y8znhomppCHkc0gJyneURqWj%2BpX6M5q2mzU7cwwNRnwbSmmChZR7FAC0gckiHHv5LdYGwnMuUFHyJFGRP93YYgXT9tlwFNFg8uFs6JfqTE1y%2BCpivJke6wdCDKywK5Jy2z9OJ4VML329rFyJ5gLCLLamlnHFG9ch59cvQEQEsVxBPpFJFAygowoNalyQY6pgG32B5NULyygB7n64vECyq8Km0gXH3t8S63YgJn2drYcr%2BNMAbdSMjz7s6Z9qnmH2XmdiutK3roBSZ8UdVSeVl1SzMBoEmQKCU4VOT8Wc05yQXMTVJv85QPSmRfoM6mt%2F82suPo%2BxSLzS%2B4uF2KKPcPnl9iAv70snETjPYziFVZTClAVXUmCUDE5o1NAd1HL3ea9PRgrCJtjgO%2B4dz7ZZXPksxTtjH4&X-Amz-Signature=88b332287debc5e26381c66c4aef6711fcd12119a65d4c2848107cfd356c2d89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
