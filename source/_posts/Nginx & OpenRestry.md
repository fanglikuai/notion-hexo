---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FHF3OM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB%2BkmZTw9CpgsymY6muL2CNG%2FfUyBDs0lPfTP5y66LFXAiBP3duOqFKMNW1JAOxixKB2ZPFir9OckU%2FA13WMRvA9DCr%2FAwg%2FEAAaDDYzNzQyMzE4MzgwNSIM83Sj4krXfQygpj6PKtwDWUS24SkKChtblgeYvSnyNMDOeEYvPvofWf8gIC0ifofaySQfBQHzaayNBYY7xxmRfth18i4rJNtOPzer%2Fe%2B8na726R8GUpN664SsDXkDzSibleu5qdFnV48Jf4kUrHL%2FnvaSDa7dyAxXguM5l5Izd2DxYcQci1ZfCKSrf2gvUsM3gGPcHpUmdvUcPsqNhO1jV0iXu1QR4evXN45n2AT5s4mpsVpAC4PQ7Ic%2B3J%2BMAXx0Mi2MES%2Bm1HXp6DemUj38bm3Vayf7tLUd90nhpwpMpAoFECzKvqcHmHw0jSeBLfcCVhFw5yDrJlivpFCy2O4lfnD72oSx%2BdsSIgJsB1RNd3gLFERgWdPedjd6FrFUz1aJGqje0c2%2FRt7ipcSiPCtSPlK2RkunmZS2JPJVOk118vN7GaxF8IN4Vf4DaAfON711ktUUHAOLyfgDqhbXi1G2RvEaXoxbjnTSe0FtLuBZHJJO6r02Ml6RHdCqmrd21gxHsIjCDOJ8AH1EIp1LBS%2BmeNwN%2BZzd99sP7aqQJrwR1WI9twZ8bijJfO0mRrHBPsL8dB9je46G%2BYub6Mwbhbu3Npd2wO7zs5AP%2Fd1MWC%2FvTBFAUJvvdB9ZCzww2A%2B8%2FhzxxKDrJXlnbOc5PsMw%2BuzIxgY6pgH8GLnj%2FhN9HAO2tskd2ueV8nn7KWeRln6RpDAxDYUQCAFX1srDc9BvLBpPtNsew6%2BHUfavBkfw9foxSYpTFFAFsyl5aZb219UwKVqnQ1vh4fW3rHM3pPIWzzLrhU%2Brjs9oACjDifoukPI4k4rm23rV7eiAHqouVvsDH3LDzXXoGH9Vmu7Ex7MAAnQIFkNcKMTdw2bSXNb7%2FRT31kk04IlpE1qPKKKN&X-Amz-Signature=c85333500e3eb781059ce03e3b548aacbb45796963348f72e0aae65f8ec158dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
