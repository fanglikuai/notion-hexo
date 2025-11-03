---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JJVYUJ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXVJiZgZPSrKnZMUpIZqSkfsz8QI7DDAlYEMlYSc%2BF6wIgcZPmJhShoEjr%2FVnLmUQGy0Iknx2aNMQbjuHXcm91C3sq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDJ2oyQwgSQ%2FYnUMpdircA%2Bo%2B3Gccp%2BBPU8Q26FvdmC4T%2BAO6o4jM61sKnD2N07CqbM9ziIDnxRspjCogdxcR1Dtv%2BEZlRzARPKii2iIRZsb8O%2F2z8lwMlN6WprtozkhG212X0CT%2Bok7rkQX2ymYGZvCEJKDavSlCEJAd%2BZJGiqAwkFl%2B6OlDaAOV3QybHVxKzwaOaMPP3Ai7UqBMU63cnYPtcEf7V0L0uu6smw8yeZZVO0uma5k7t2rirU8SXcOKemnampvX8Kvw9S%2FNdAvpz1Rt4d39HXNVk5NzuyOOGQ3dPeQ5rY5TBX1uhRfqme9cMncPzs9kx1X%2BvrOh76vjxVn0QClI%2F%2FG3BHfqnbZQ1tQgJhZVIrn9vy0nkT9P5BFMZWId3Ud%2BxvxX2U%2FKYBJJwyeEM9wKIcVSFm8Ns%2Fy1EMMia2RaGFxsq5RtPRQl49mxfkZ8esBFWVVa9Gnec2YGqOjwFbrFthwTv3rFiNejG7fHIFw4hlLTOaWH9Eegue5U9JpZOlNNgyKNalKC3wM5mLgXzCsyjKBPth7NyXpuiqs9hPJAeslSjDycMQuvSSdzuXE7IwceW3iMEnJtG2Sswb0q9I8NQJDVIN9OegV8fm9d1NNBS4MN6HY7DzPn7rqtT1HakS250xGONQDxMJW7n8gGOqUB5AxnQQqBSeg8DUcYZeaCwSuNjUMWQJn76C7Gs8wItBiPiGx7JJG%2FA2FV%2BidktxG60ylNDLw188e%2FYZ7aais3P72b%2FhoCqSHZFJ5sQ9n1nAsneCjLvQBOC8BceT6YNmKf8WOo4%2BCF2%2B%2BJJzbkdnneEVV1nNo%2FkqFTNDTaUODy2O1uZKzcP%2F6B4ricpvMSsOO0Zey5o6qbe9NedRnYWLdlB3rQC40d&X-Amz-Signature=d2af7e6c9acfcb9a2109456b2f444775b30f1d1f6ca8c9b95fb83eafdb46bf6e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
