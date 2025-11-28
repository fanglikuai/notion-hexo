---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZ6GNMHS%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcwlOAcG%2B3tKoYpfC24m5ghdAqzRIh%2BDNQA%2Bt4%2Bf65uAiEAiUZtRcqzzJzLHo7sq2JSj9cB4XruLfjt%2FUa3eKSuxScqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHXIwuJ1X9mx2b1OsCrcA08CYn3QmkvwcxVGG8aTKpHA2dvPW35anIeNvMPH%2FIEc5aNTMhxpeH51ziGn8Eo%2FM9q2JlttbRyD%2BxwxA%2BsPi8J6ulS1M9F7FnXKcRh%2Fa7P%2FndkrUBwRbx7eT2iNhnJ4TTXNMty50ha8GO3GyIA916QqBJiUnBwXlFaxqV%2BUuQ4FBjk5UJ2nwdFXhC%2BbXbC4l3YV0UkUTmdKvgKs3km5b0fox88gnpSFTD%2FgP0QkQUYJMygycCgaOHZbpGEnsPI6GqTcqZXAfC9xn1kZwepGrfJOaz9TxCEazGx%2B3IeOj9Yt%2B%2BG8ECThaw74XcjDOD1WH5dzBUB67P5WULX%2BrVGjpXxV7TlSZpYjU4UyoPiIpJ2%2Fcrz17s4h3DjwXNFdFOu1hNMTHOHxik0mzQX%2BdDaDLdQB2c4Snmg1Q4EO%2B8XL8n0rnZLTOWQV4vgwlYEzrHw8JTz2GWDPPF%2BalFTQzt2ppMCydT3ZbmK9P0sqbJnOrY%2FO3dWCuDxp0vPk7fHUrksQs89ZhlQk5em8cu%2FGHnRFmpiR3SZWx2MdNeNHfnFdhxrERtYvOSphA8XWi2gGz9vHDHT6hxDcNLh2iJPVP1X7QZKIQaMMit%2FJbk1CSJLn2XI8P%2BYSaxzUGZZiylwlMNO9pMkGOqUBAbH6NfrxE47yMlGT2JOL1wrcNX%2FytoOz%2FXYHV9asol5OA6TqtAU7yvuyx%2B2lPBkHUb4H%2BWXEGV4c1lCJEKHK0Es7AYyS%2Bn2AVqmIzJ%2Bv2qzIU8TgOVUxeLLm0JNeWwSm59C%2FAc4W2mMCjCDDlsggtcMcwql8Q9uZNHPSNm11ea2gKgF8xi6FH%2FEc1uq4uTJdef0YpRSC5k%2BPmCo3ATPxdHFCjC8%2B&X-Amz-Signature=f65906a8dec3185f59af9b83882528948aeccccce70b5f825561e7052a9f2f1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
