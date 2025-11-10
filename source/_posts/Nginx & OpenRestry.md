---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRGLFFHX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIEYYjy0cXdW%2BEMXpvA3sJSceknnvfnQOZMG0IMOLaGIiAiEA0UAo4nkWST4iOj1zTkzT1oH28s7fazG8SuB%2B1Arh9cYq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDBQFIKQ06QxYFHTzHircA0261kYKW%2FVx5HvSddeGLu%2BHRXQRzPGgVeS6TbSIUmVSf12UklR2o9uMnGw3hNAi5z8w3g65HDFWmqbXWQh%2FbzdW4POH9R1dErXk9m9dFySLci7qTj5RoPUtO7KEQtyHqGvtXW%2BFzGYvnvLHLAH4vlpoSNwUYPry4yDFzYfe5XCrjrYDodYhBYm2988w0YuxD3vzAHWcOET9tr4qx5J0QXStzTiB9hgBOeCDEX2nd9L33j3tj9f7jMlu1NpQk%2FU%2FbBHppXfYC7eZWWiZ%2BklGa72ueik9iAgZ3uV%2FYTOV7hk3QVTJjXORRRxN0Z55BHaSZ%2BKn9AYwIEQ44MF%2Fy4YmmgXxvllpnbthHh9h23KUIk%2BO9SV9h4%2Bz9De0xa5xqfFIilePoaSMQbb3ZOaNAGhDJEr2tBSGi1uPsvcHsFrbFqKJaF9LyD%2F1X8V6PfCY6C68f3PnG0XKF8qYkHwGdD5%2B8Sc5TOZONGSIkGJyORTY59Uh%2BPg7MrsyrfRuftq2%2FmtO9AZQsDigYCLTTO4BmM0l9J0LK8u9amE6Wd%2B9RHhreTcuCbF825hbp8tCYdylUNXgT0p7Y115M6JwUG5OL52kW%2BN9Sopkq1EP6tklzGBd63j3obrPY2q9Rb4PUIluML22x8gGOqUBxlWv%2Bojxl5fwBNHSvqAIJUzzQm0zPpzpZwt5JfaxkeIrNKJnQqeSYCcX7Y76SXSUEbUbBHt3D6bdKlQsUNc7ZM8K3DaiK36tK7xUJ56lH%2FdTEdAcBCEObQsPaPAKa%2BaY0M%2FlJ9TLuYzPmIGWap3o9hTam1HuhZ03BPO8WVbrNEkSe2uBBXZjE8LDox4%2FwFhYdShK57yUTT0BX37iNp6X%2F3GDN%2Bwg&X-Amz-Signature=7e1fc1db34c4e821e897a9a56508543cddfb2ab36f10c9dd391df3948c563784&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
