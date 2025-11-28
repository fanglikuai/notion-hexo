---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664X2CO2CL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBIZu3VE9KuvP8M5k9H3gX%2F1xVWWdrK7hWDfswfuM7hVAiAXsnfdBXuAMLOBLpgvIIrOcfIYjmoo0r5UsW6NUbJnjyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfrWvkCBVifEnx1KUKtwDETuTbWe7ENrBXHvHZFwVwps%2Bqe5urSrqzO2OUr266CpNO6wXkTglH%2BLhDdoi5YI6J2ZQQVZxrABs5INiL4TAuse5HhPleH6SvpZUZ550lmFCo73KXXix1T0Fk7%2BgilQRRCbd%2FXOAyBKjnGua4ZTXDaSChDyx2VbjqqGVmDqj%2F6vkxMMXzzXlOvpJpA6OvAKgsAZzRCZ2LGEZD1JEcvD5Az6pXWSDh2KcFE5TK0TFsqicQMbG44me2GpufZnMBYI3RCVCUjs0zKZZsRGLpN32SWTYg6jaVVuMgO46FubImP45cnar%2FgY4hcDMYg9ZSQUFxMiqr3yrA8BMn0F6L2KDpOho1b4gMEY%2BbXTaMb3uVS0rXqF28t58fgwJiwuR6MAfSYeToM78gHPjg%2FuN3p4GhMMknimHIVvzDOBMhOu3YHLi%2Ba3S2V5E43%2Fn7tS58EnD4yf1hClcRE6lFOviUK6QGyOtKdXxUv2dyaPCogQhi6mCVH8yCla2uBxezaS3hBVbLSvZUHSZo04Y7z2FSe4wBzMlm%2F93eUaRxwMsI2I5nf%2BiM3tgLnxG3%2BNHRryIQljW0ZqfM279KVnXUjcfBTzL2bDzb4sZOWaO1nTmTU424eedt6lJfWLlkqju1mswmrukyQY6pgGFFT2z8bn%2BFGfUJXCN%2BftPU018Du%2B9lFT0oSAP4IDiQk%2BXE%2BNvp3buwNIlwm%2Bb7cJuNiKhZIzwT784XAUWlZFe4N7%2FHoNMocSLbUmAMcHcHuzqlua0OyFZSu%2FWZc7EQiPrXGWjDR9oAKqFb4ap%2FE1Napa2nnjrdI4l8xljOlzXKOwQzenXpUv2zvBLrwp4Eb4hQ1rTapbe07u2jQmeRvEUVH57rjvS&X-Amz-Signature=2f6f4e9de212019e4997943ec95707edd24704e45b231e42abd1ebb53b37d00c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
