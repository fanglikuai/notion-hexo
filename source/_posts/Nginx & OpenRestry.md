---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ZGQZSL7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T160230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCICw1T36siA8K4lt3BJcykH6pQIoDcm%2FUcPfwpUDEwL3IAiEAiymgYdy6yw4KGoOcc%2FRjdDCoyK%2BKevVu4SiBoUk1N9wq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDMOo1M76eHi00Kw2xSrcAx035vNtg0wOFwoVn2vXgyIYPfG1U7HSnUhyyIVYFaMsTK%2BeVC2dZqnDtG3WHbLos6rgkQcqw1u4suyZ5QVKhAattGFRriRMsY6bLl9KqbdUumZnGWlZpYQERqYzMerjeP48mt7j07B7tPBU0WNsJZWBwNG7PUhxxFJ%2FsGI05y3lt5xP2WccyiBGOHMDm6aoKkdqexC5yLtnnCP0Z%2FDVdMQKBcCpiKXIL62pnHiKSfpeoeZErFQwKfOpcZwXZ8sWqCcGdYONPkjhUB9c%2FaZhjmfcRlctnAUh%2BMHfsbol%2BfvQCOyUz3iJw6jFEa%2BQgA8n2y4wpOkbVIQx6vzfOvL47v4wQZm9MdZyRsw99809N1FO74Ig%2FsCF%2F1Q1C%2BT336oJx%2BxT7dCQ1IPrLFsferzQN3NLR%2FcpKposjff%2F5FKp47tmGyq7RKyvLCwMQb5dyqV7WQAIz7Pe0%2FPH%2FI0q0TFYGEyxHurYkeYWgUeRJlZzC2s9pMErg6CtWTaOe%2B2ZyAnYXqP8ielLy6C0oC%2FSZS2aUt6MxNLuT2kwCzdg9cXs5Cc2e2aIdHzNbjJwjihYHYkjQSer3RNKHtHG5Fn8K90KxQqaaOfRMVjQfzx9wtM6yhmqrMcsZ7eBqXJSKcrfMJi6k8gGOqUB9xVOebYY4MN7sRXly5fPHaUuBk4NX2yfRihDMMH7J236ogfttE87RDf7AwF2DLWWEDmhCEVseH7I2Vv86QSVbRHixV%2FghestT0LPT7YsKHvr90Ag%2BwIxVMaa0k%2FdZXMGGc7sUWN%2FLwM8p0778Eu0V3L4q7nbLLuxWdnIqAzC%2FsZNqTSNpZgqLVmowY3I2LoGLVz%2FDl1hZvt5IcVOhzD9ceMycsKm&X-Amz-Signature=073c4f9e1cfb9d5f628ac4e14d7e910b19ff377ba6b42eb98495f55c4f4bbb4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
