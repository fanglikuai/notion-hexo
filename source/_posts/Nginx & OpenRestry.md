---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHTLKU6Y%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHlcPGfNU8U59Ay1EEeZoSXKtbxH0w73dYMtgRoYCeavAiEAq%2BPvQgZRlNcGymfByb4sYIRNaUA%2FLzExdE3dPcJdFXgq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDBUd7AHrimbWabKEBCrcA33apEwHYO%2BPqcbkN5FbPoBvcVKktD5ZTKbk43AzJ%2BxtCVVlCNC4idJRzd3LxZQHXSv0CrLX%2FqvKHhSVjOjTRZcWj9bnnN63G87q8CGzQkznzaZrnJP4piGgA8SUp9VkxSij243YP5L7zN1z94k3fC90JQDZFe8C%2B%2FeCAyjrHUDETaksuXUrAghLIBwPDjd2ST%2F6%2BQzOjaEdOlE8mFs6IQrL5JrP9%2Fw8TLkQ3A%2Bd3x%2F%2FNHDfSuP1ue7T06sJ49hnFAD3UhUu0%2BKcbdzUXz%2Fc6ydL8tPKHK585Y0KYlgCKR%2BIqQ8PRw%2BkE%2B6v53okEISu5RvusjDpLswYhKdckW35Gg2NO7e0E8dwOkazngBuT2uLxitVGll3uhEJvzNcnFCZo7Tok5ruynJicDWz2JrTOj3ht%2B%2Bpb%2FOavoCCnLuUsuQF6lT5Scg3esyMquFzU%2FUSXdsk76O6S6IeQSEdQNf6IZbPzgyBWfezbgNzR690XAFDEVgUVhqMAwTtBg71xLV5fcSuL410qhhw52AONtJ0lFBdOIGdepRamJfb2lXmmsqLmCNNp0fxw2UY6L0aoPAV5b%2BXoeCR5XGXJJ51L949jmjQum8bJ3gLKv5SyAXDWx5NN%2BqFeZF4WLXSPz2DMJOI1sYGOqUBgd67G2fd%2FwE7SBHmN%2F%2FhP5GrIPeqFUGcK1IxU6vO7Icc4SgkUH3O1BtbLMbYqVHv%2BxF8dRYWyfMvpnGxTRMTz19WW9UFNlCb%2BIvt%2B0K2uHuthXUgUZutNGi6TspL%2FZkyuPY6O76Ok%2FY3sELmMfwiUDAG4IemRBE6CFKs9bXmYua9YiZikd8y5dG8KS4Ade2Z8Ic6qMj7cOtsL7%2B0dGGGF9rPY3mZ&X-Amz-Signature=7b1e9798b2b4e3c2d8cb738e1f0ff3c196a6cd8bce66e6fdbb121c6e98f2872b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
