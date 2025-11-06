---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUHKCC7L%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvX%2BXPreYLbTcVMZbZ90XiTF32JMtV%2BAjggmGO6AE83AIgOM9l9jCDXFcPaBxaHkqUDrK%2Bj1LfIe3Q5VjPJQjRAtUqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO4IfN1PzeCMFPQM0CrcAzUOghLBdZiWwBnBpyp8368LAcsEjhSafXtsjYbIqWk0swly79U%2F2b2UPCMOvoQex1doEKfCoHiNt0JDjeVKx%2F%2FkzbMqC0Rw8IqfTgNZ8M9rXT0oCz0B%2BcR8MoOc2%2FWIcA4VhgNQFoghKutDu31qC34HbHdvtK%2Fvrr8PkgRPdNSjQhYgmVu7e3hSOFNA2WDUk9vNZfdxRaWiqgHmEHuuDMBpy9j6%2FaVv1htkxDkAnOKOgORnaVyDa7aRyUvWPamvMmy8kwExwqfCqbrCJRBeLUW1p4dO4JlW4tK%2Bv3s%2FGK0VnnVIKSPI78nW9QzdTp%2F%2FatUUEQhikN6rnGiG258KxM0WlfZHZ2abMjM2GbLvn4CUC4q7FknT3yWIrh8SG4eLdsvGiJ0TTG%2FchBHrUpMX%2BB28O1lLWsJatEmZ7F%2FuwxjbtjEeDnzgzZ2IMDFZItD8N27bi%2FzFxWyHTo9%2F0HMpf1Rme3hr2nNvsplqH6R%2BJ9uPT%2FEfSQ46MFUm5trJNd3TGkcNEf5cJ%2FoRmbJVMB2Q2psbxZjvIx7CGMYj2HdMO6kr50URLUmVjNeaF3v%2Bmvk%2BGpG7D3Rp8V7q%2Bz4ptJDLHqywpb7%2Fgu7UGWq5MWPcaqZ6PaWYWF5fsu%2FpO5lcMKe6sMgGOqUBOJsN19%2FNn1BXG%2BxCoinvWd%2F%2FjKPNzBbBXfNlnnC%2FpJGKc6NNRVW9aECnEXjmCl5U33VXwM56azk%2FXhB1%2BOYWLmNpdwbNVdoyjaZOZ62GYX9Cc8ARQ%2BHMS2tUi4YclB9tAH6JzlRbd3zomXGwB%2BTMWRQJG4RdlFMZluOzIW5RPxrlyL8%2BSBaeBhHZ9KJAqkUvjnMiyj7Tqn%2FyVjV%2FwBsSKdEVnLsz&X-Amz-Signature=d7e3df84e1f921620dd389ee59f1c950f736bdf7d4d023f0a3a1fb2102374bae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
