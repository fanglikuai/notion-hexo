---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DYZ54ZF%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6HpvpTbC4feVVWBaQp%2F%2FZVfcerMTPrPt2k%2BSj25U1eQIgH7tx4rikaHVsxeOTdn6wGX0KgvcH0bzObxppNaQEBLQq%2FwMIIBAAGgw2Mzc0MjMxODM4MDUiDOTPMBZoYYjkl2z0XircA1VHkX92jL%2FZRk1MgmKgf2sFrI4uF10X88z3smni%2FMW%2BO%2FARH7D7XrLsdoRvPd02oN1QWz5WXLrNMVSEj6Q5MCI%2BIEa%2BHGDZx3WYHQYHRoX5C3dRRx3ejJ8vj9ygDnh9W6cnqEyVBJUUxVGfhw9IeBGTT%2FlGbME%2F8D1hAJ1jGlac4Xb5WvSBjfeUc%2FIlhRAX49vhH7JUSvprj1aLLCFNosIiMffU%2BnO07YaabBQFcnr104Jqe4CPKiXOdUdp2kRjNz8QibhhwrLW75ktioY2xxUgDNbshWvvtdEFnyONIDLXUl1R72K1uRDG4PqmlgdB1s%2BULvaa84K%2Be1cFquAKNpgyH5sUj5U%2Fb4LekbArSjAKtxjgKhajwNz9LvO3trJMockEbV2myuUrrZJEMz%2BkuUzrMFwK6sTw5lov8KDPdvl5Ak6EVlQ%2B2tr%2BAOdeZTol5ykBH694%2FUzVP15wi3Sevv6yVBsVHUXq9ui9WH189Btke2HjLN%2B%2BoLGdZcT9RxRfbLEyfRu%2B275n9WdoHBdQocoGCl%2FjOl7kQEeqneBTz6TxmG1qiuLSUAFH%2FtjQIhRMs5R8qi0odACuX0sWQEOHI7UOvxADewmMfOWXmRXrGSPZAuf7eSUi4%2F8ETTIxMKbV9sYGOqUBdsuhfwU8P8YF7HvO6N%2BtUXhg1Zr7OQtJ543j23hsnSwOkzfbNZR%2FQn2eRrKP0LTPpDYF8%2FDtQYmzMJvRGAWEbbRARvujX%2BfOj%2Fj6BsBeLBm6bXD7Hb2ZUlQbnGhXkDAasW%2BaEUkPELTZOHD5lfdpYVIB%2F8gkLCsfOzp6cjn7lPOJGLlyANdM1pCHKPFFNw1usmVx8SNqpHIv9f%2BkBM0usyvYkF9k&X-Amz-Signature=135cdee8da8870e782b5662a4c310601494f7f842007faa005f4e8117ba85239&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
