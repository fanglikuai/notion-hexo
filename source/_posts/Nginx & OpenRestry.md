---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQ3B4NV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T180054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQDP52q9PNTe8zWZ6dpYR45PtV8vTX%2BxEDhGsbHizYgpjAIgGqvlYnkH18hPXaOpjkt%2FCvjJ0kaOG4%2FUi%2BtIPJPSnHEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLutctyKGLIP0xFxFCrcA%2B2SWAIqoZE43qq0hh9bKKMSTf%2FgWElERQ8PBsvGiWm389ztF%2BxcOIJM6xFw%2B5Uhgax9wOqtvgxTu6kerq59uzNNwN0NtGBguojEABx3KeHbocAkI5EkZshfPIXrhG%2BQKK5URFJNlbuU9PytANlHbysUDwztaaTKeVqccJ7WJ8WxQ0IGPtxjlAK9bZPG%2Fa7xMk8VFMdMHxwrOORQtIVCPwI0keXrJtIo6D5X6VsHI5ts23tY%2FySQP7SwVywZC85Vqql%2BCfC1jU7JpoTaROHFfRicEsg%2BiEeSBORo4blCUPTl5XQRHftTSly7fmQfVDN8Q61zWzh0lLNM5RH6O6aziaM%2BaVoZTCbe4GPTAJdWIlQ51A6If0gAD4zMnZarpkQddrAT5%2B80%2BvTDdnrKYNHmzbgnWKvz1Qo2KDyDuST2VlRLbVVKJXt9eP%2FdzZejSOxMPvuW2mHj7kOqZKUbXmAE%2F%2BIorH8c3L6C428lohG4edQnWk%2FnCDu40aGYAfzfFjvXgV2mJEdIJmJFUvzFEk%2FsWYDT%2F8%2B48gBFu2LpRgEkixOxYR4D6LvVXMstgMgbK4rgS5lgIUXRBI2EbcJE2hSxAYVz7vbtATsQa4%2F2IDlOCtP3a3JCR5dtZcNqozs1MIvJgskGOqUBSj%2BqVURJyVXjLhV2RFOf%2F131gcvqQnkQCSr1UVwjW%2BViftDiJSpD7uy%2ByN9BAOMgQ4JcErKe1ZBfKtu8yOzEqli6Ni6kRoQkPxBikgyfVwb%2B4hbK9dbfIi2qbrJJRjUmziVdTq6%2FOOJ%2FctdOlwGAV6uPjgcosXWxaQsMxNVnq%2BUMvQ8fDWw%2BqgDNdHVniu0dy2MxIIY4aG7k42hHswtDNSozdYkh&X-Amz-Signature=89d05aa32242c23c7acb36e75cbbe095d4dd3803e96ef7c01974e276581fbfb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
