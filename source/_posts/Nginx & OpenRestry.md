---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QKTU2PN%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T020050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC6LOv%2FZpPgDLuEFK%2BX0GCF6snRe0%2BK8eH4Dm0sYg4ywwIhAKr4%2BvtrLcGtWXwb4JksTCT0SE9cbb6jcr5SJuTXAB0aKv8DCGsQABoMNjM3NDIzMTgzODA1IgwJVe0BfdNcp1Xxpvcq3AOZhYWxX%2BDp%2FgOXZTbBSsX0WVPYvuduSQdEhGLJOuAwzHAfT4Xkxv%2BI%2FT%2BzZWA%2FmdWoonlwUPFAaWN9vETSlJvWbtKfkZvmiQczt13ulXIl%2F5IoKd%2FREd8moB7ULtNPBvp2d%2F74NRBZp77TQLnrsTd1cXvNQ5ZvOfFnEXu6N%2BXF75bDjwa895yzD7YdevlRZRkFR5kjpyooY%2Ffgd4LrzQ%2BM3QAJj76A7TV32d4siKJhCeCxAlHV16ysnrbZ%2BEYeL82dxO1c%2FdcOO3n%2FS66UxMg22oDx2VLmzUod7FPfPjDotnWG2ITaxHHuIsxLy8mn7BiP%2FdX1Ub4p%2FzZfnbLF56A1JBt%2FEZMJEE19Q7GLFAL1%2FSeLAEXh6%2FxbDoKAng64zc4B%2BANtrf%2F4G9NtMRfd5pLAyLZ0i5HSpfCU1USS6EYgrqTx7Ho9XkjX1aHCiF84GpWC1DIKBHPVA0k18xsSp4xHn9ldMJjiFUx80wkEgzoHqhPtruroPZUEZiQ7zYAULejt25sP9fNbxhe9KYM9amHB6CbilMNRGKbgYSGWYTyTatFlUaLDysm1HCI2QVyWiUS6F6ATwzhA6onPL%2BiUOmR7WlYRttOGaQacORUXJyhG%2BnOyYn7kYtJiYhmUMTDU0vDHBjqkAfmku5Vklt%2FVagXw6ptKLDX0ZWzsY5sGQyV3n1bnISKcJB%2BWChpO4HIFhAn9NT0dajpRoVt%2FIuJEBnBwQHUB8OyWGrVJEa8aB0gYxYvmPnynvtwn9nQaoYoZidUfjjGAyutPP0ozaSm9vjLoLrcyXmzeqEgQO%2BmJho%2BuFIKXnKTC8Kma9DY4ebe3Ub2mAN5bXjt%2FTFz%2FwIHC7V7hDga0SSHb0L%2Fp&X-Amz-Signature=e48a7563bc1ba6da77b468dfe73ff960eaeade3a72a4cfb3a55e4b6b3376044a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
