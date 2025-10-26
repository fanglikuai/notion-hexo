---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLWZC5KD%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCMWhNqupK9Rc3Uoh80BV%2F6FPs2uDHGC66jyG%2F8hmKDsQIhAMChhBB5Ev%2Fi1WtYWxXF%2BmvPuLXH1u5hXUVVTDMe7E8yKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyexT7TPfuWM2%2F5lRsq3AP%2BGdwLIzUPDhVnDiZ9rkR0bKfsFz29CXc7Qc92VA%2BeDo5x2V5f83x0JLAoDh1OBnN6TSbxJkLh6eeZa1Ajr1UHrZInHlPWPHDmHs4IXJPIpIjy7Plj3nbEd9KXJGOTlBOWDCSuIZyYT%2F5VY%2FSvKEn1NRGUiU17PieN%2FjamQLD0MfFbkf5eHzaQvJnMaHQvlr7qe5yvix5Hd%2B8jr%2B9qv5t1tQjIkEtH%2F1dTFmlJk%2B7EC4shz4dlDagjURkTdZc4xodzep5uTmujouCT%2FIjy8CbghKLvZTFhvcIUaJkXcHD8cUnc64PXSXojeLSWNzRc4RezT1M9nJvEcXJTMd20tK3%2B30ix3NIG6Fo8fkZa1GsU3mUWsR5YBgPTtCb37nePDjB4rOE6rte7%2FqMOIJUBbx%2B4CgoLlDtNZPpfVjyNN7WDVyWji058NxEDMVMBRaY1V9S706u%2BWHqpKV2fC77q1Pbwy27m%2FZgtEmieH28%2Bf6a7j3%2F2Kpb232TPM6rnSktulsaBKtJQrv%2BzrnGpxYT60Xmp6nWCxeg%2FcJatNuzVEHN23f5nCkyMuGz1xwqG4jw0XVaE8VagWMUiCctqgaRkIO%2ForYtNujCv5QUhFC0ih7b4wBrZkZEADz8h9LlerjCb2PjHBjqkAVRxf0z5BZSQmf8MDq8OUvbDpN%2FXmXO%2B7ugE6w0scgvyfWexU514cXFqlRkcw1d3LubEOad8rCApmmjHZWK5lNKU1hQnyLwyH%2FvgY5aNSZtrT7exF4KBZVZ2wmFUh6xO9j9HlavUv5C32wm0Sf%2Ft%2BeWRluG6ilIHY6ZOR0YRK7IUgVejIvSr5oXh1DVleY8aMf4QZuj045CXB1nbFEJkhcsOWRC5&X-Amz-Signature=1bd53404126c824ba544681f3c8cc0a25cd40275aee7937083e18f8b2264dc91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
