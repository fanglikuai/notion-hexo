---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665XESB4BK%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIEuBLAft5nAgL5xidRkZPHyahdoxx2AEsdFcmiTqvNuLAiA2%2B6S%2Fo6xMQAi5NjEjCFodrkbilGDugRxEljrvA79DMyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMX2GraFFr8QdDTDBfKtwDdUNTxNYi6bHY4VFBMlz0ZGPcc4mY5hE2dqLFRtD36sO6cJW9e50CnLLHd7uDvXOHN9pLQuOgmdf2JpOQ6GAgYPigBb6O0DI%2BILiq1ZFljjrUO7ZaQJdvbPHpPx9xWT3b0AgTV6MVVVXQtNai43HYIVVxWTkykP%2B%2BR%2BdZJnHBuNayYg70Tc5tztT7%2FYKBuDlfmvUxE1zXFYN0Pd2dl2DmM8NoaHYVrMNw22ZF0EoP8bdsnc8DiTCiUwzJKrwCfbmtEQB21H49qlqsi5iuVhMvBF1oIGEr%2BN5zzrtFi09HsKSkAiiz97tHVTqJirLc9%2BtZk4h29cVAIhJyvJ8cWEphAVmwzbHStQ%2FphR6Kz2Rgd2%2FeEak5pFXQc%2FzSa4JsWxEaBL%2FX%2Blm0WHb93T%2FuhWJYDXk9QeKbZ1kvhSbNdsysOwWmW86oCK24Sa8nfgo2Zono%2FT4X7c1cJ9jiUQOKq6Au37eA1mC1Hlz4m2oblKWZVxytyk4NdBvE%2BcfX5Gahkd2YaxKgoTUZPniu1V%2FsMzZ%2B%2F3oZ7yaDh2HuLHHIkPtEzKtNrxOoHcWE8JQrL%2B2Rq%2FNpltaurP4e5gJ7KsQPAmWfbtGAZX3HzJbCaT%2B%2FHMVzmSFw6zSjrSOlK%2Fe%2FUA4w2OjgxwY6pgEjBCE3NR45kKOf8ZqkredyEOy5MxK6lvC9f9%2BG%2BJ8yn4LbNRJSjCiQq4Bv29pn7qf7xIJWmlRgBzO6C2FY%2BiNOBBMJka6tY924jbp7uRTWLPPF3Td5rectkVsjkFLz%2BkY4ICZgEWNhYXImZ9XkocSk%2FWx9oPIDcQhP49BUj9qZiiR98KB%2FbFPjxQt6bEvf2AFZDBzJyYx42fBYCqZIerY9acXx1sqg&X-Amz-Signature=56588c9f951f2bbe529344ba967de06374d55b3c4beabf59d0fbb47f10ef78a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
