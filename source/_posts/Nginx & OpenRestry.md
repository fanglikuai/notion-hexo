---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKBKQJD5%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIQDotwumjP6BDJzxMN6aDH5JLgr9iHZZ68Vc2sT727PlUAIgIEQiV7u%2B8Y363Zl3ZpWtRpeRN4faACylzLhvB1yzqFgq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDLb8sGbCVOJdXTUzwircA2iCiqCNgXhmBDwnDrJdr%2Fnz5e0VFbHDqB9%2FB06E2vjwH14hiu3GHQcgVPz7w0UG%2BWG8siwkqMJj6nVy9iONdJNW4J4oRw4%2BNi7R%2BrIto5jv%2FPlUXCzynKI%2BxwdvNbyafwSjWrQ3EeoVst0d%2FGkBiDuHLxJZNzzF4WFm94fg5biNX8oMyUq%2BdZEqS0Z8UFPCHLIcOfQRHj%2Bq1JXT0%2BF9XeiHtf63RHwdWl8c8FpWL8vi5p4Kr25rqkd85JSn3TrbrfkQzLv4YxXsJstBZY7xjBJDC4bGzxGmcGs5vG%2FPiCd1AM1elSzFrdWwbADmvIFWjSNcotmnxxsjvHmnGLYEzTA021kTIawdQCwZyQ8W5Dc7Zv9TyCUZKZIo9cUUtOrKB567E4OjG5n8Fig6YoWhIV06FxkZitKaFb2D95s8pcG2P7%2F3klwt71czvJKTAu6xSmn771yLByoJwwUUxB%2FWphv02ElYlJdiRyz1m3Wjz6WB6ZpBNObBg%2BvoToDRYxlDaVO5qQZX%2FfY1Mz0T7DaWl25KA5xBBumy99uZN6bLV%2FNcIvrgbxGgEA48pBQx1Z3igY0Tn6cusb2eE6u0b9%2Bw6DxJ4FglynaJi%2BOHJmhSnJCSawiw%2FvJVKMWM2xLEMK3m1cgGOqUByGiwLxtkgwvdLENT1dFQOAxo%2FFX4YoQnYRW95sw6yFZPgJJoJP5Z5NHRs0eGyQgfPgWSlGuc1eWHOxBcrAiEfniVAGz7twdA%2BO8marsH%2Fwnjncs69kLCdLnLKz8qOfSqEcRLKywA9CwGrS87vFoQB7G9lGxXXyMSuZP8MCGIyzW3s6LrStKq8DhF8xbyP%2BVzo%2FguzLUznR9lSkz0P8fRK5PI0ORG&X-Amz-Signature=28a4e2eddb82e984589e67f55f165c5c508763fe4d33c099e2ab0eb8c1752d6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
