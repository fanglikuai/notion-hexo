---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJLUSFXK%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDN2%2FAOcqEQPRzCICGu%2FR5PqQpMHtsBkL1EABKC8QLECAiARywvBSajorq1dPdsi9GEPGsQIvwfrARrI7Jq4hmBlPyqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQARf5FdNZXY%2Fi1LrKtwDjIk%2B5TZZNoaaTcpHMc8YF%2FQoQ9ZhIGdovr12w1BJZXajLXdFftYd%2B%2Fic1cpv4KtTKs0SeohzJBtWcAQSGdETDSaglUVbJl%2BRcYMHwTa2%2FCC7bJqsqVe6G0CNaq88ZOxOsbK7ti4tUXncTB0bK7b8Fn%2FUJ504ZoS54JT5aC17woxE6iYPWjCCETC0%2FHOFUH5eHawwhVg%2Bbu4pKhoFr1BgeEiJdCT8v9E%2Bq%2B1yFM%2Bj2RgXHE56EMPcRvPhELocgWiB91dNikjG%2FE4B4oKHqW3btA5BcN%2B%2BJ9e35g8nVopUnX%2B5kpA%2B6%2BN%2FJcAF4whJI9TozNSxZ9TS0%2Box%2FRivcvQPJhNNpojDa7Z%2BwgOQ%2Bs%2Fc%2FCMQfcFig9E5Ar5J4Tlm5AyZC2S5KvkW51Xly%2BGu5Pc9swY3dqDsN9558%2FYdq5SdNk89gqV%2Bk%2BxUjq9hx43Y6SMmlgwWAoyw%2FbhVQDppMAOsqk4MaY5EwlAxgSUujJ988o%2Fjw%2FHkVIAjtTWl7M%2FwPrNOvDL7MQSzYnIQQmEuPb080dsbyUVOWzcTncVrDiaROpEi34jTo7T5mVX7qxaFzIu%2FyDfvWHx2ll2GPJLx9aOM8fq3JrngTZrgXCXZvmCM2IHK2PvlyfSW%2FZngdAEwqL%2FvyAY6pgHQGupMk400mlVTI12N6XTuBpI3CdiNCM6k82yCk5z%2BWErHT2iEdwGkzx1rBkuqzynbrLKmRzgm76%2Fdf%2FUXnN7A3wN7cRkeZyNVXthcboQZ5EDnLu7L2lsIJmDxmOHM5CKLKmOt13H1pCweFjG4S6dGt6eDc8u9R3jCwx4h7mbRTGacZSXDXwOESLd0UJz1GPWxrRm9dmgw78SsFdkQd7GcUVu2qGVp&X-Amz-Signature=a70bafaad643f6e8721d992bfec09cf05f8cdb286fd907b16f2b326dfcb47706&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
