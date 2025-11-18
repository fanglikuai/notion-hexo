---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLLL4NGC%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCSMnS7ELCinKn7uSujjF0vsXWsBPX3ZPswbFOKETQFRQIhAL06DHsUEShARR%2BOOWosw60KYCKaDRcIC8yftIgyak4JKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzyz%2FP2OmUGhr43zeUq3ANL4RpHvq98Xre%2F%2BW21gzDgyH2iR8JO%2BCz%2BNDwUkIHndWDRRgyeq%2BVtgadcNldZPSkFLWW8rDY31JRCJoc%2BRNO3syyiP7GAUCq4xjLvydh1HN8UO0%2BXytLZV6VkIWpbQM0%2B1cb1jB75vJ2%2FsgeYoUefcCQWpef%2BTqFK6h7HURexgQeW6K6%2BsScS0amoChYna1CHuVDMwZEdtq%2FEl%2FN0dh1EDmBdje8fb%2B9y5JmNxn%2BvcCOkyCw4T0ZvqDPLthJY3%2Bch%2B2iVcmsn0zVMySL74oJxzqm1hWkmerbtQNxCPu3r3BnB65jLzmvWMiCbhMpKC9Oro67en%2FsfNI2lHPlV0BINP0kV3KR1kpLoq7zm1pSc0M6fw1EkOhigz5PvlnJSFkWBmo%2Fk9la8KqmgWWRvz%2FumBIiZ2XQ%2FIf6xbR%2FSE13EJKVZ00i%2FiXdJawySyiGk5vBVBQfL39jw%2BvtJ7ArcMOqzYVAud3v4whFT0hX1tBQCvrtKpYI%2BqtVyI8LFnwj5dJhezm38r9b98IwPtBPz%2FkekL9V31r1Icr6w931jumPlzyNZjdJtuu13I7337Z9y%2FU5iIFL%2BuTjN%2FLd91Dygyh9xg8xtSmP0a6F4gDNJ9oShOVNxVzHYT2CjdPfy4TCmiPPIBjqkAfgb%2FEZEdqOsh7VqKhCoZNe295lT0tr9WlpIsHAm0b2u%2B66K35WFIf7rDW6YOkhjsCY0Iadq%2BKU0eMGq%2FYzro4khz58zdBoBGB98nehBzz1HbcNon2O5R3kku3pEN5a0yMvMaBVBn6Akj9cG8%2Bze9JybIJBgWYE%2FwbYb5O%2FAxye0fk%2BEl0p%2F85kKQOThp%2F0of1nG91loAX8sagbfviPXnKI6JZBF&X-Amz-Signature=05a9b91037450b2970e46f4bcefe5bd8658a460a5a66753fab54ded59b5cb07c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
