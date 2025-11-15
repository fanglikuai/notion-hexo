---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE66G5RS%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCAhm8RLrW9pldjIvBGuWfUZz3ojZU8tGAA09o7bwQ0QQIhAJ%2FQD4D2%2B0AlcoG%2Bc6kAtUOkeydri593g4vFrpSlFVN7Kv8DCHoQABoMNjM3NDIzMTgzODA1IgwNTFXtKvEqssW6a%2FAq3AP1ns3q%2FIYtSvz0KmzwBgu%2BV7gwwOJg4NHU%2FypbgaBHNc5oCBU2g3rO%2FHqOc%2B1m%2B94n6kedt9TptqJ470pNNbtocdqhMMuBQQlb1Cs9IEOMHqRisMShRULM62bsK8Lk2Zbc1kimQBORMuOfjRS3FHqYu0Rfzcm86bancI03YqBBh0v2j%2Fpc%2FgNCmJjaqSVQcA5%2BsDHadk2Pc5b6WH8b2XihkqNEJRySXa2iYo05MAp1m9y1gYFyfcjy6vmyT7x%2FpPm3RSloAKqTzus4An1Z7xmVzLazLQUFVWMOImDrBI2CZpYSmc70hgA4uTlXsJkwawF67LIMgQ5DRfYx0RQmhYLS6f1AA8fVCnmGRf5VovtuP54njNoUle2mn%2BQaBTf%2F40ktL%2Fk4QsLH%2FPqSmns9f%2BG78zTGDPH%2FDYcbuTvAkCIurOrX0tBSq4SYt15pCMPoUR7I8tzq0Y24dt6KGiFMYwIuCX7MuzOJR1ofQJv2xzb28xx0EMNfIWjd3iCuAqeXE7k9c9juAYXza5D8Vy%2F2%2BFJL3Y%2FSJizys9x63BPwnO0tWer%2BrYL6E1S3%2FTDAfUOUtxF0YVXLsC9yNlgClnMqAORf1HJbZrezloYGzitVocwPJS3MeZE6vJdVDaNc1zChg%2BHIBjqkAaT1LKNsIbi%2FKC%2F8%2F5058h%2BqigRi19DWq32V5yoOdpAfO8CdM7VfhmFUrxG%2Bn4PCZtETOt%2Fx%2BYDF4o20drclVHyf4VUIRo7j2Sui039ot5zQcbjUbG9t7l1Iu28RZ0Cwlj62o%2BKe5NAZC%2BTHLKb8wOSf9lN9%2F8zvo38YuV%2BUVA0xSNdmOZZsCeZhabVmcpSaAE5517Qxo9ZHMzfa%2BgAFxVDfwgyt&X-Amz-Signature=ba954df0059244d9084f222d8c0790d372e6d6906b7632d86a78cb134e09f4b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
