---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HWMXX4T%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCZq56M8IyIRu6rvIWM0eMRxnn33edh79AwQrabVV5UVgIgDBKDE8SlG4XvM4plcYbAoEzoFj0DpISi0IKgjO6AzLUq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDIsUzF2Lc6uENZV2jircA87gW1qQbfxT5QedYLkmO4jB2fwBaArBiQ%2BqZA%2FDUAQpwNAiOT7H1EeinE72v643%2B0VcQXETEEV61nM9DQt4AGx9hvyXpYaYh7MlluztRDMyv0p2eD%2BaCmBUOxtVDIEyBe8uvbjWzITgL0mGv1ESks68pS0TDKpCEp3SIq1S%2F2iMLLH76lGbxZJwk4vcjTsSoPnl5T5Vd9PDQOtqfebvvpmlYRPLWjVi3ncgFGZrp%2F6j52sqkzW4ScXX%2BYug3LhBHs99nxwaML%2FSNrgTqqbSVYhSkJ3Ef0%2BJXPetMNLtmCW4Om4UDulsqVgcEp%2F%2B%2B06GU7DOnzPqjIoK%2BKjgaZQ4KJsPOafatEuM89f1aRrJFCkTbbglc4oLZQiD9tYxo2F0lh9CZIRI3GeGHn7fjX8kGhiVkMfQe4Y06K7OcRGMP7RBEtjEwR18Ifb6wzYo2UhGOu2si0VBGbSlJjajIBOLrX9F%2FUTJtcoY0ly0ynHQQyh7dDBApUFXOCRMPkl23nfYt4m7IbB1FYEIP69HKFsINQqQYjuW%2Fj3b%2BipvBPxlR%2FaNBEm0TRhp5JEQPEbZ80fvrZN%2ByIcBBGMb3uTJhfL0IkpD2AB7jyuwWkr5pg3LZRStdefVR2xn6%2BFHMVC8MKSZv8YGOqUBvV8Dzydmvgh8NUUXQI9W31v3it4WggPHmOmKsp5AVmraYEhJ7JB6ccfAldt9g4GO0Es5nRLwH9pi3pSK7Uck%2Bl57bm9T2Uz%2BFWSC20zRYsr54OZENFKnPm1fgW6gC2xsU4zII2rtSdFnmGHmheBcL4iCqvbO0XXcQSD0pP9P%2B1DSEuJEQAcuMcjKGfLEU12%2Biu0sGQEWw0LixqanjFkTZmugoLMG&X-Amz-Signature=4c82d134332d55f3d57d1f5aebc24d119f8f285c16d721a6c476b18c5177681c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
