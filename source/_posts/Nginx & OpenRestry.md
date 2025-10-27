---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVI63XWM%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEyvupk1v8RVL7nteY1l2xe7%2BEY%2B%2BHG%2FpMY4FIHoUOryAiBpfCY%2BfihUhyZMm8ayctmX4SnlSla6bsQqc9WkD%2FDRUyqIBAid%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaxQRgVSAbeUWpNjyKtwD%2FM%2B6AUJv17ieGCf8Ldvuj2fG%2BijoEQs2ZNg2HQMFglhyKwODmPSVUoWJQ%2FasbAjTYr1V3S7t90By%2FPab6OyYaD1gwhKTTH34Jq6ddc%2FLPB0kYSxNfN0qisI%2BvzZxOrI59y%2FRbFWW77KLOIdINeDV5InCRvjIpq%2Fx4k0k3%2BSmDPGwlHA79qC73wQ7oGbP62ja%2BV3duG3v09kzRt94mn%2BmJvkF37HqDbbuyyGZTIIJ5X2VXKUBPIzGuz%2BGp27op1xmoA6sZyV3BJM7hL48pwm9Hex3NIZuxwzq7BkoydwjImDAvaakr0Yhm9Bot8BdLw3dgTePejEKeo8J%2FBGCtt%2FMyXXDnTVxSScyPUxD67V8vs4HFbgGmJpHeXZy0c45Ifkl3jQcIs7%2FdHl27SzXbchS9HDMIZ%2FI%2FRmbF%2Bo04RZM%2FkkkvJLF1fNYhEhpTqQ8wduU1vcVF1GRL%2FMcYwenMiP%2FN28l8eE%2FuJWrpIgL7%2BAJ1lSqhFCuZO3w58mJG510Zx3MupPD1Rq%2B1OLTJZ2NBO3vHjLZE21frJR%2FxMvAkThbPxxJJnW2oxdgdV8jWO3xiGh%2BzJB2Uv93R56BTD0%2BpZ3SCR3p09YYKmCxnwS4vEr5eSEd5b3cBDveTZl356ww5837xwY6pgGx3gDCD%2F0%2F4T9uIT5KD%2FViP%2Bf1xklt76Sxcbi94IoODBrfo5pNPFTQPMo7pEGE3G3d5aBWzsttAYYY0qRGv9uoZftVQbsI71SfdrUSWCtqR6%2Fqrj8qqOlzCPLTS7oJeo%2Bh4%2Ft1Nyomm3gTMEJUtqVqNkODWBliNE7dZ73%2BfOHsrWY67ILDLgLJW9rC4u0%2BALKuhpiL79mAGKGt99F0wM8FHVT%2BRzxC&X-Amz-Signature=a6819817f919301e0a844fd6959bf33e1a92e9a9366af68f7f648ddc83a8a196&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
