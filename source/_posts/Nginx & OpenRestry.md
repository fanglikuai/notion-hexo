---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666F7EGK7D%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BVSgG9QFMfxSBrAnsY6IB%2BSBIIsB5QcX62KYgCDvjHAiBkvTb8ESONeccwP25EmpLrArp0gASwohOJrNraWlSUmSr%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMcfCOrjUGSYrYOkWBKtwDswkWEbLF5zWFt6YmE68g0ESRPefp9pGApGTd2t%2F5sdSpcplGo%2F9oZpAANBjMP1789yyevJu5eE642Q0IJ4wOInTenArnPLwzUq0c3ayoioceWoFBxbZhCUeqnJ6NXJp6DaQGWGamSjfacHor78BC%2B3kZI7myGkPgkt%2BUMdPrMDk0cx1VnkXV2BXbaSmxmcv%2BJSxnG622%2F5MKsYf8f%2BfK%2BUEJQAmdcrG1IM6KdQf0UYmFj51vk9YuCZcuT2zBDES9yA3WCHDpaKN1snCJQKa2wxrtlW%2BScfTI3JtOFhYpg8EKXd4wXHhGnyrMsW5GZvWi%2FMQdwl5h1%2BFmUwPF1J6XkBo7I9TadVj%2BOIACR%2FpiHuTARGez%2Fg0hApvYQnzZI2vIlZvB4R%2FKtEfNQiJcmW8wBoBS6NfjiPXzwWuadcFCyt92CkjUXOCOWPr6DX0FfDurAcfzsW6R8ykPz%2BDKgTJCqnrD5JFhMyC3v2vdOcHdlYoy%2BRvidgA5glyATRTOCuj%2B9cj7Rq%2F87RQtZPYFLk4AfEo8IRALy8fCrL%2FsIlR3QqB9akXmtcLvhSgUGBqdNbvrgMiU5WaN2e1PkGDhauByVqH06Tt2cR4wxsPEpMkC6Exg7cZZI9zY0zjF1kIwgZPZyAY6pgHPP0gsykxpQTRVPrdAOr6ZxE0qTubTCOrs75yy8FQ7S1Gt5godkpDMTY2XTRpgB%2FKzVV6KsxdI%2F6kvH9oJm4aw25Sh0CWwyhkrruYhoIUWrT4qJQQ1fJjCPyteEO8Nd96Kr1yjmOswkzBN%2BHk2S1X0W%2FnFCnKlzqK7eJkrG93rQlRRCYpMopn7mwLV7R3efKkk6hUJEE0m1FbAOaNuqg3AgdYiAU3J&X-Amz-Signature=af9c3b57020cb096522df9481caefb6c32a45f04e9fb0aa5c10cdd3f2a6cb8b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
