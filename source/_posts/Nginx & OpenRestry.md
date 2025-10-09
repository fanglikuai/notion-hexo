---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VEVZZBUT%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDUECtOvawN%2FNG%2Bnrkweknp5iKY%2BX4ZF%2BKcGvlI0PsCIAIgQU1hkb4tVtLYY7ph%2FcAJNaGQINFTbOIWF8iseIEXnJ0qiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNEqY%2FbKGQu7adOPyircA9nCBwi62jGWdUn07iabuzLCT0wKISAPq%2FBqM99FVTCSDFh4iXZKJvRxrJZdO0nnuUrREZijOe2X%2B%2BXmsKMX2c5O6oOevEwLZmp6z%2B%2BtCXDHC4Iqx%2B4WD%2B3%2Bspp3vZ8E%2BGPhsamsQh213qtlGOHpg%2BDVYne2P9cR61xsOUo%2Fa4saP%2FBY5ahluV7s560dsZ1ioe%2B2rmboEUp5M%2BOtPjFZcamGVtsCbTnr2vCcLxoVbyYhJM5UPmc%2BHqWDocRWMvoErBlygamppc0SANquLXq9TXWEyIO%2FEqtxY4sXWr66nHBWxy8U3rwBfaUhxiLaq%2BVYE85ocALf5xToACnx%2FZrKmWVz4jzkbdP1sxt82AfkNvtnnwqcz%2FPbepr%2BuURp%2BP%2BRtNSfOKtFog03jImsmEqTDCYi6ZwCXyyfxL0TidMX6dDT9n8Uc%2F%2F9Yw63M7Ar5lf7eLeG3z3tMXDWR8sI79Gh12%2Fx0XqRW1%2FHc5y%2FS%2Fl532TQP%2FUJ3a4KhphTib1Yck%2Fjew7qs4MQ3n%2BUt0BAnLqMtUBnm3MK3JDea%2FaQvKI7Mgz9KX9ILUlIgnAI3G2NKIYjNh%2BFp9tESBpiYbCX5cArK9fW3umFV4TYvqHPu6h6syETPRm840fUdxRMY3SdMOvFoMcGOqUBsSUZjtH9%2BnPffN9Z8HUUgZlGveIk%2B%2BORMr4SxIkX5i%2Fg%2B9drYozdwbvG3iZZFjqvw%2Fm77F15m6M0Wc1PG7NEMigWQVmPn62l1X9%2ByO4aar0MiaiRcvQ6wWBBYXn0Yn40t1yXD2%2FkdMI9DgURXEgr5BUP95NfOiewSFBNTqT%2FL6KTeQhmzLSzUEX%2FqHbMf0Ef630E%2BFyWxaDFqU92xGPe%2Fz9Tcky%2B&X-Amz-Signature=1cc281ebd0b59dd3b53ba016e06c9013ca5c81dfeca3027561bd5a6c1711564b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
