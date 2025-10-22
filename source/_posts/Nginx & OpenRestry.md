---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPJLOQQM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIEmyRUKDV3N5ailaaQw7N6gqo6AplnHKSgZRSep63%2FTvAiEA30R3rUr4cU6Tm6WqXq1NE1AwYswlrz%2BpY%2BfZhoSqqB0q%2FwMILxAAGgw2Mzc0MjMxODM4MDUiDN5MENrXFX%2FcQgjSXircA8H%2BogMw7Jf91b7%2Bph5SOGdxNSXRKmNMHEW66HDo3zJe4vfJY9EhL4W6xjJ0NsukPtH5x9Gpc1JhqXIeWec3fL6odhk37MqsAQPun99O8IQFfa7RqxeZ99irRqKbn1YhRVDUaoS2csOSY42qea%2BhFAnPCH%2BiEvpxQcIWVU%2FNLMsGIY%2BmaRllBG5bBnxWJUl0cZtjPiVR7rxi3yzFYayPuu34CGJfeQF1tt%2BqJ7E3HGtEj6vKMcSyxFuZfphhfaoW0%2BgS7IEcA9Y4YYgf6zTDOOW8ftWhAwYmHUp4EUte6HkPwd6MVFrXmpIov%2Bu5pcuEyl79V75gruz5cjwBmB9QBMEHNCjMYBQtZv1Qk3EFf2Pnj3Y49cD1Xr6Ts9zlcSBWvbl8khMJ1YdGHN2z2cc%2FicVkMglnOlDG9MyqVlcvJrLLecqcgBZMsDuCqqD9PZ%2FhSfu6RmP3uRGgVUcO9Y%2FMvEAfR0pqlidfEzqu6qKCoQRxp505LZOYD%2Fpd8e6OQna8rK6QmCUD3wJPYidcQHraxoMEanzzOQoeEYYiFvAyu85mk7qU2PV2CQhiVQfqcHSi0to37PuGYk6bABn4Zd9Xqg7o4js86ivGw7yBd%2Bh8DC745Ag9FyV9Y9JVOhfyMIrK48cGOqUBZgs2XLVO8vuoe54mGjEqDd%2Bomeih6x5Qhjf%2BkwfLbtPZecwjF5%2FqqVXJQ%2Bl%2BQ%2B7XyBKdy38xK2yJfKLK2zAkHihhnhVp0flQl2RI9k7srvIjrIrS%2BlL%2BzYzEx6xdGczEyESjoc4kTvb22XbMcrX1zZlCN65prNUkW2ncimnNQ5iM6KrrSnvzNFSmlEIQoz%2FA3xdKp6ubFfV6kmmut8mNT4WuxfKx&X-Amz-Signature=61be6bf62c952bd4e303e41af979bf6ae7458cef83d2357be2b3bbc0a1cc3309&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
