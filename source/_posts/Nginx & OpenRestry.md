---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653E7I6CA%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1PsLzrufQ9Z%2FDTHyJopIFOCm3GfD5SzLa9zeUJ8P3TgIhAOM9zYZxu7ijjMacalftN7lhy3bIZkp1FVMpln9tJa1SKv8DCEIQABoMNjM3NDIzMTgzODA1IgwFuHThthz4cSF7C9cq3AOTORvbePFL%2FnCuS0IalEyzXgLTsriO1UNYlq3N4eNu6r2EQex36ehP4yWbUZ%2BM1BH7TOlSWQU%2Fj9Gg2cwxTMqJp6Et8sdjhprICiO605kpsbAefUHMwWSVSwfPSlNIRR5sJnckKx5AblxvZy1ZTsoo%2Bb2fXGYrJav6YjPeHrDj8gTwW2nCs09zzJHUsaWsDT0uRd6%2FkZZZ3SnnSVrGIHzfAQk%2B8QxdH2%2F%2BnJUEqzDpjYpCvYabpgm%2FSemNsUVFXBVWzKEZIXvdllBhlEnYmT%2BlQSIUZEGFRgLuKVdaq60lzflON6ve2H8T7Q7jrRJNRueheKwNzMMml7OsAxtyN7AcL2aD9oKOmVkQ64Hllvob%2BQOQ79eflwLwolo5Eqf5fcGa2nO7Tz25a2E8dANQCHyw%2BnUQxK8k1GoDleklNkj%2BLIRy%2FuT%2B988AY%2BF9OKbMqF7B9zeX70noIKdc8MsneeLONzvC4Lk60eWw6zzxQV6SX5HY4%2BLqzxUHbgOZRerNVJSaf6CaHxUOA4qA%2FI3HbEQB3piw3TLlxOJtXY7asO6k7aZ%2FAdd6wME0SBPaI0FGXtqw3H%2Fhl7lc%2BnksOWRCN91eMJKq4zRxeze3QAP2uCEktTlVBT3dj0wM2sAdIzDGgrPHBjqkAR7Eg0hTVwsUC3tOE2mx7mTEobaQAjgtNf2NVXBEE7QExA3zQxOKyOEuJ3CZU%2Bmc9JSqYDWng5AVfrUbo%2BDgFipZwhpiyIqaXWGGmHB3SACuuM1Dg0h6p%2BOgaggiTm1TnTV95Dt2tq9%2BSVs%2BSl5xUXzTyzYiWH43Qy%2B6FhPVsNpkVg3%2BerblNMs9TMVXQDXgtO8esh4d664I77Gc7v8BpvA8hGLs&X-Amz-Signature=56d7cd156ea32e4f01b00f43d691e7f268f645cc6415467e346a794600ad1a4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
