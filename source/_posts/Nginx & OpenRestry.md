---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46656PNBO32%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T080038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJGMEQCIBK%2BL6MMsFrWDsvCO1JDB%2FLPh%2FURdebWCmP5e4sT1L7EAiB3kV9%2BAYLnVM19h07waZ0UUUQMtPjhKQWPWho8GGpmhyqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyqC4rLvERAL4KuziKtwDoHWqPdvrEJNBnNBmJ0GPDLz0nafVJCqmCYae5nR%2FTcQVb%2F0YyL9570VgVGGIZX1V1KG3nZnu3i1pRKvFkmuuOkuaPEvEzitUNp5kiapSlaseRCHwBSy6yDCXpW9daea5%2F5n%2Bf4l0xypgp7a6NG7cag32Olb%2FQ9AdpC5PtgAoQtCTfKgce6HCdRKPnyvHjUTbytHBr4zAU8clrXIUzw7RwEs3z12gumBXLMZlPVQkPOx7OEujBRYiwlQA6NrL6xHLmcZg5bufcCWxa%2FU3L73Vmvy6kTuChBsOt2PsAqybqbKqN43zjn2YX6Mlz0gKyKyMYzyYWLr72h20aVmFztXFUM6KWh1vfhtlXgsM2baVKGJBgOJ32vcq73O6SdUSKgeyUkRQLyahS%2FcqbmnUgSWU0MKM1JoEqdbFZiLmp2Vc%2Fvg0xfQj%2BxduqM3O8Vdk1ky36RLMa5IP1IEbaOeBf888FvU1KKyCAz1%2B8YrX3vwzSzL0xCclGj6jGhGCSrCvOb2i1M61NyUoyOSnWdcB%2FR%2Be8dyKI3Gins%2BqCoSBy2PsOP%2B37z%2BK2mssXVwVkINKnTfLd7p%2BBnH%2F%2FRkg1al8ij79N5f8%2BKAZvEfEiX3vSdtXSRJy5m8c%2B8odVXun7Ncwu%2BLoxgY6pgH%2FtY%2B6wxthb0h94QjGZ8Q9in8tQIslm%2Fv51HZZEs%2BXpqCu5iDkRRa8hPxHF4%2FqVdTWaaQePxYc0WqHmztMfD3BzqHkS8NKMXE8eX%2BKhMKUnuVntb%2B%2BVUwK8u7AOlFIHh5PKf93ZD7e87tou1Av6HSxBBZLkZmPCry%2Fhpcc24LCy9pKn9TKROc6J%2BNj4MUHBC6G29lrvAkCEGFx9TBBtnTaNhJ3gjM8&X-Amz-Signature=13b04f78c10fdebfe38148876f8472c3aa714d00e366e5fdc4f34b7b7ccbe666&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
