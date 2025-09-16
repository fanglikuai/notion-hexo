---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667NS4BNMB%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDizhGb5SnEUhJBSW%2BHrIVpYDNueJ7nER%2FHsTmkT%2F07hAIgdjBpU0vo61KmlyHvuJLxjXLjjtJSuXAA0ShmZCTfam4qiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDrdOI6oLQ%2Fr2w%2FKVSrcA7kRm3akmp5NwsF67MjNL%2BhU6eNUEp139Tq2nBAe378YXcbhzKTUjExGdDqy9Ir0Z%2FkCCB2jIBklSiQf2Ple9HNtP6EMLwU3ztdR8IEujD31A2Un1IxgDhpkoiIpf9K9C3lAqBiNqoIW7YTBy1D3pDdpZ%2FkqT6yAocU7b6TJDuWYzNCYPDHmcBoJZ7NUyNfQGTv4EMPHawerasnuD4DankhWurKl68TfFSd7UjnexV45LL1hhBE3C0Sr2hHx9Slfu3JobkIep8dinTV27ttnH5E%2B1BQFMcl8y1Srf1iyLxD%2BXJ5goeP1eCuX1LgMKl%2Boa5e4sn4NAGpWAnfxZUHh4OmZXp9wyM3gMccK%2FyHCZq%2F8JzeBYOqisEH2KTbmIUqSeo4HTVuHCLj0IT57uygD47%2FdEN4UBrx64sx%2BC%2FaSlhcKm%2BBR%2F1gSSSlDNPZDAfg2EkFiq12iXCiUhIeJD2Q41t1ePAJ4hNsGHCRL9JhScFS1V%2FiSko3Uaj4TZBrsyxLArHC8vtBTue4y4fG0vQfZQ5UDi16M%2FazFFDOHnc469p9cMW6c%2FyUzs%2Bbm9aDuZaK6pEkBP1mISXLbzaDtC8rWSpU5kecJcmpBmarKLLHP7Vtx2kGBxfC7m0w4fU%2BmMIzDpsYGOqUBsSonoYimvFQCUXS8ArkwvZGPpADwki%2B60Snv%2Ftujud5r%2BCRyJTskCk%2BZfGSsRq0lzhBs89QLzTrrjY%2B2c49C8Ix94b90KkSBSSh97RfaVmOEweZhFg9AojJqXZdTPEvayH%2By03ZALw7JAbu1RtTrCHm1D5hG7eypqRUo2luKw7Bbb%2F%2FmmSoISbHxgjSgTHJ1%2BXPJFH3T64k7i6m5p0Q38ztASD8B&X-Amz-Signature=66173cfbe2cd4cff39ee79b74736ff0c88d6906a19475876900f50723918f782&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
