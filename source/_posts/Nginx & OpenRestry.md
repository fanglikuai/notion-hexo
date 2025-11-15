---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMQMC4FN%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGwaMa1yJ0wa3ay9faNJYsIBtmXTb3xm5dq54tkgKtyHAiEAn85FF1JNY9h%2BSqCN%2Fa3vfbbGKsfRUdvpgAXaY%2B%2B5SZQq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDOiJmE4kwCSI37UyoSrcA5vYPnjThvsFDcciRNqKB7DMkPRo6d231cDXm71I5ut2QKAO%2Bp7NYk%2BsT1nRV0rjeseqj6k%2F%2FJHg%2BjdoSxRsrdJh3I%2B9gGMsourXafVF9WhWUZ8Ln9HzZT1ypeihaerI%2FT%2FIpY77a8g%2B64AghPYTuqnS8XBs8bwbnbOp8yLqtd0g1ECm%2B4Kpc6AsCJdQdJ1A6Hn0gHP9344OWxk5QVX%2FwCtKmLFCuNG9%2FRQV41JjOZ3t3cDsiHlSvk7dYTr6NOjDW64%2BLnMOoymXipLZ8GG1PHTY%2FXhE1SQsRlB5gAmgjPwj7YldIEw3D0iYvQLhKw%2FuP1ib0QzCu3EKlajds5h0otssoK%2FUTi2GbFjKhSuwaSEDF7O1%2FQZgHrZHTbdUqxTiaeeHot7xzlQttsk0SxtbPrssujhsEqxjtkuQTlGdi%2FcpoBYlsF0TvgVE4M7B9XCwQ54q78BohbOMpWdBLfAm89TkZ6nqJWEVYhleTHCWR6BVuP5PGzlsFcGLnbXfRiAVoQWuSMV2j72ZMWyPGQk4DvgE%2F9Vv5Et4NvowMyvmFHgzq%2FPgWl54NQhvaxNtt4zzJo%2Fto8SWdb0F3ZW%2FsH5aCysRyjwxnlLD5mVQPeXs%2B5QHEvqmJD%2BgWI3Me%2BZuMMDo38gGOqUBJZviKTaPjIwex6J2O4iIvXzVt3troGX9ihevoMsk2OuhugF2gzhOX2IKWo5aQQ7kc4wkIjP9o00eLVdV%2FTgWaXBgjQ8N61OR9Di62%2BTABVlrwzzcwzodzNMvZzE2lcKZztRZF4tc%2Fv5Z%2FvdPXc1fWTiWInrl6tYFbdmUxY1DxW64A538qvq%2FzgXMZ%2FpfvB43O6jWdJBg3sW8iK4yTyAtX33P5PbB&X-Amz-Signature=324e82de2b19107a5fcd272c49cca8b7d6afc6cd6564f480a9eedc58b061a558&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
