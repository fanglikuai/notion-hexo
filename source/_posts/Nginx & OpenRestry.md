---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSZS5LEA%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJHMEUCIQDVXpDKmoNrWTuNx0JZODWB2nVA3tufYGU5REKnsHvlNgIgW4w0oalr%2BYHkTMLyBVhjnCsR0AlQy8XN%2F4TC8TtMcr0qiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPwGVd3SYipXmiWgGSrcA3%2FgntJKPw%2Bdf4hvNAqC3C6brL1POnkRmATMfq6i7z6wdrBoPaYo7oe78%2BzXXD1%2FNJlhc7%2FFpFHpoSA122nR4Iy3gf5BsttPPmwbmoPxgLszGelKe2ZSme3bZjQQD9TgWTVSa%2FSca1No%2FcGKT09LEi0wmlfw06AgL4KsByaax7BsCKQI9lf2J8ghOBuNOekR%2FwohnD1bAsOv3YemdVffnrAwXgZJ5A9m4XyaQ5yty4h%2BIDIDD%2FdBMN6fC8VrJy1lvNI4Xw2lNQORA15iHKSAtrS4APZ%2BD2tuqO0tacNgDVfVBRWEJsqXuUlJF6f2gHHwf1tkdvppwwOS1HaDLtlfhl0%2FM5cjIGYAd1lYAB%2BOZkvJZFTWbS6Op9MyowgLr7%2F9dGg1sGfpRuJeEN3D8b6XMEqS71aiDI%2B1Xo2t%2FUWdzaMyqM%2Bpk1CV1yCHaLCIRpjoiEQiy8JOPKFhsq8YV46hDS%2F7ao2My22s%2BbnX2gwlqpkVnfZyr7Nznp%2Fl2RW1q0RmTuI0elcTNLiIm0Vi9ousFxRV%2F9wH2YsIyyLdYOEs1Zfp8jiameuTWIykGhwe1zTbu5ec8r7UdgO95dZYyQ%2FhfU%2BoGM%2BZNBTSqDPiafLeiSMc35UhvwaT%2FTqPqk8vMJqNvMgGOqUB95YgvRLvqf5Qy8f%2By7JBrJQn8BsGDewBx1EeuCTUSSQGW95SSx2GiKCyhpif731zDulG9J0SRjUeUlibMzMDRchquBR53Kzh7KKWHtmPCpbHjqXaVlWPuLww079NgLueoR5ZPVITsEnMoRLWyGtkZ13ULUEwaNubWkXhr28ec%2F2cGc7bTBwMmhAuydly8UtvP7fsIsSlnpZLKWF9jsUjRdIhbePb&X-Amz-Signature=dc9680e96b71d3073ff8e84b5f2ff7d85682e67ee0777438d88e672387079c77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
