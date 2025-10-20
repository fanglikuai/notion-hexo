---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3RHRLAX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIEUuZilhdogSr6D1mVZlKJe1KEf%2BJKuxaJIFDuQu1fMuAiB5G%2Fsasfcny%2BTvAsdJ4jS1BUbdwesm%2Fk1dkb1SOUvr4CqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPQnYaev9BdhW8D7hKtwDcjvvAiZDiyYbAp2zyxQ921jU6LylnXrxhSoz2o4ZyPxLaEN4x8polasjF4xcCryfsq7ndRYDjjz0oyTBIAn1EQZ3SDHUjbW4JJZplU9dDf0%2B8HY1WHvOHrr7l6BTE%2FV%2FPuH8xPFB02egzu2AMQb47DGtI1SOfbJeJCoROXXYuKE%2FkmcQIG%2F9TuhXzuTzvkJXtE78aJKDgfKiJ1VE8SnAZBmDjCfmu9MvCcZff%2Btd2h%2BX9ZaxTPzuGi3U%2BhRwz%2FwE1TQuSbTWhEw48HAH7%2Bk%2BlcqDGNiQ6Z0twQ3H%2Fwo3wXQZMGAtv8xQjOSD5sBWgq%2BdZQ3KBlUuFVjWTRrAoZP9YEfll7nnq1f02YYRqSs5ssyYdF0aXaaXf8aNWD0ZevXi%2F3OS8BuHeskZSp0GYZp5tO%2BW1EM3urfAJazLveL%2FGpwtNNjYgmBy15i9kl0AyOLHSYkpgjTlpXlbqsVNHU07LI0PEdYRAxBrKyniaAaLNM%2F2%2F6%2BVr7HT1H0YJMLQtcPBHu%2F3Rn%2F%2F56cEjL6xWyA6aeKN0PYBA0BHMAGZRh91DqdGuxZkQSuGWcMAStxl8g45xW3beAU7pZmJBi1S2JWZkWH%2FwowIGTQfwvBQF3QX3xxUFw795J0lYSg6YHAwmdfUxwY6pgH5jayAeBrVEGSvn541Lmdl%2BN%2BWtAbPZDu0S%2FsBwXIIfVA1pHsPQ2NF3pmYBZaYUpS%2F2ZLnycZAK8d0Af0mE7G5C5jIchi%2FYw6JQ7OdJfpGZZPpD8wjCSTJvUDTOJWQp5pO2dg%2Bktw7dl9jJcS0j4b5hoYvRr9imTYXlO20AySpM1LjmInJN2g8uNYZ60QUl11f49E5y8K%2BGbppYOQk22p%2FXczzmea7&X-Amz-Signature=806b2a1f809af51943c1ba4c4cd2ce8ff7ebb5db2ba9da3dd162b228ed2f38ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
