---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVGDGEWD%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T020148Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCICWOqKNj2tvf8tkrn8RpP3oxyqGEddsbdFqCjkgMTEgBAiA%2FPVw%2BFCX9ReERd88ACppK7lUI4slhvYOmDpmc2n9muyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1orxCkR8XnFoUmusKtwD7wI05W1b36lSuoIwlz3yD9vICVXsmtZFzSe0wPH4aQGg2bUnKwvCOKi%2Fl61z979%2FFak250X0ea1Njvml7TgCDhsmNZq4jSQnKchOU88K0icAem%2Fs7kZbIYkpdHJWFhLRdLtOUU5Qulc6SLY%2B3fl3KjG02zuTDwn2khRgH1KajiCTr%2BK4Q3uwe4%2FSutHn8VQcaI37UjVk4UXrCG9mrGKxDUue0hqK3LvfxwFSTxnBORwjHe%2B%2BZ%2BJpI3I7%2FrOJI1fmEcWAorlWl8dVygGJ6nlmdIxoNUY576LN0BeItC0o1stgdNOyO23XUFoYaYaHF%2BO85NnROrpzIMlUgM7%2FRd2qFdqnJX0uEhfWo%2BxLW32RBx%2Bwn%2F6rheGmgVP4MGLDX1jC3ysPapwUFrx3J8uSgCyP5fNkbkdxfP5kSQSBUqH0nM1wYQ%2BvgpO%2Fp3i8wq4YzvyN8PgWizQvtZ51sin%2FJ8b1y3c%2FslGRvHQp7WGGDI1g2LNp7UuoKIMKSpMYD19tlmadewQ5NvsEr%2FjyOYyHa%2FbwB7nmLW%2BLJTi5zXxV%2BTywDvyLgj%2BM3VDAF3c3xLdRvALlNWhNEshDUMQki3wJRFuR%2FdJH6N0IFyMgmAv%2F8998lb7CwyxUH7de7HTe0vIw8qSyxgY6pgG%2FcP4C5geOdrNMDQZaz7%2ByXrjXDe7purF%2FCar3kN00JfOrhIGK%2FDzfLEQL2nkmTwNYwKr9vBoCQXFfQneNkEW6FhSqqh7qYHmXd4PXGmuTT8MSjg3COcQ9FSHANirsfHRMV3kHQQJp%2FA0AREJ%2Fq9sviAcMjRFY40G558%2ByC5W7ekRcHlqIKjg4wHm%2BQfTWv9ZXA%2Ft4vqlkQBkXW%2F8PZcgUcx%2F6vjQz&X-Amz-Signature=4fe98de284741a96c89fa91264618db6f3f0c0eb844054bc9a7464174a598f40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
