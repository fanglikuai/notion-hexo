---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNWZR2IV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T100649Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAlyV8WVAAgTreZciQnoRzZPBT%2Fjz5fsqd4ufmt2Yjn7AiEAs1LavMa8IowoxrXNbk914z5roVR3roHO4uA55kHfa7kqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDETky22Q6Ro45I3%2B0SrcA57AR9Jw9C2rKjeq52nMlchSSkWezIRERT8xnSV9moTwcuPgQ2oOUXTJdwjCb%2FOV0Mwc5wp0z21aHpRREGhpTPBMWcjbbhfpVDm3Mglel0ZIAgP5Cl57mmywVjz0axqkf35cvi%2BjgUxE873uULd9ebzgLtRleaW2p2cJ%2FoP5WCvCPTBz0tbzZ1GrPkEBFdNA0CJc01yQErev2s44dbid0v%2B4%2B2EZ4HK%2F5LcA4RYFfFJml3k9xExXC%2FOo%2FVCmSt4pkUwCRmrA%2Fr07j6OOnnd8QWiVX7O0qVHM4LjhRyKC7GHY57rxnyYgR8Ke2mOTHkGmeGqX6eBx39R5Anzyl7aMMql48mQKO2QKkUuoLgQi50o0yTK9FQfKa0ssh7OVed5s5L%2F03Tim6sFV9FWvQXEJY34UgcoLLvBitNys0NeJTYIjLxO3eMjAzQzUeB3dJRcn72YF4%2Bhi5X3eq3OvC5tuw%2Ba6%2B2rXroCtQ09fCF2WL8QlQS8cHsfJQHm9OkjDBhVU8Bo4eUAJvO%2BNWaBoKXR5w7d%2Be0wS3JpzBByR5dhEd%2F7bKNh2XiuTczBrOHzTlCjgWno8UPZZBf9aS97CN%2Fkbtcge52vxXRt%2FZS5vL2KWIfwCvNQB14%2B%2FF6zct7ciMPa218cGOqUBBUNT3%2Bo57yrEN%2Fzi7TqKPxPnMA8UttSzzYy6ao3i%2BAYAA2Hqpd0sruQc3a8iZoZGjM31WkhGbi8cfa82d3WtPTlACTv6wWiW3vP4lSM9KEeIxL6ZjtlGolxBLzji70jeMylnL7bniE6QNlibj8949bbyuyX%2FbQC50IuB7Z8xvydeuhFyEGIIWimszKPdL26GDR53PERdsFnoluKct8Z5YuCxjdH0&X-Amz-Signature=5c8468c193f0881e5913f29c4d5b40795b2d9259d7b348fcc373434ab507c820&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
