---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBW5JEZQ%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSPl%2F9wljBKr8erFNFdcRxe62HDWozqntuqcJbicilCwIhAMqj7tkfYw52kF7lWHG2Ajzbois0Ipz%2F1iFRtYXuLQcpKv8DCFEQABoMNjM3NDIzMTgzODA1IgwuBqtu0PQGOxNNJLMq3AOWLzZsByrJ%2BnHlix8wgaGfZV4tAEQxdHiiqNuGnbXz1PDkdn7wpBEMT7ZWj2uKdokMWCpXuvHXbRczd9G7G%2B9Vjrp9HoDsItJDmWl0G6zBmMuiWSw%2FhDBT8zhZFLIR1QBivGy0IGwJGy9%2FhQYdaj7qd8PmdXmbi4ncvk5PSXnWqxaA13S%2FbnRcgy%2BjCIs9TPQiPU%2F74r4ZFTegJ0IxheJLOlrzMGV1lFpXv2ZtvnM4J4t%2BPD3kjFELBe8uXBkBo5q8Lp3fo1Ead2Yvn87D7t%2BCCTO0ywMDK4UQer1JnUm%2Fo%2BLGcQd607mRE7gcEFvsYm%2Bnu1TekIfulUxjTtpO9C1v6KOIbZtNicIdFP%2BmzhGcQeWUas9xtI5y5WkaGQyD5yRNqzVmPqBz8X4B%2FH%2Fdr0mwPZuSufbv60SDWh5j8aWbTcxYVaTw3pcoX4TvnXjarFmUu%2BpAeeewjIr1p%2B6lNbPs1oUxK1TSYr6t%2F9oww5pAVGWDhkz9wAMykinNIRGEVzIhMG3QERzWAA%2BLiDmFrBn5SBqPRqLZEFOVBgcN8D2TgX57X%2BoADIsAj9iPs3kxbmb%2B8Sv0KiQzIL6vWoy1RkzbkBE40T4uhBbvG6Anys4Y1mB%2FBBSPOZ7xY97aMTCSiuvHBjqkARSLe3LZcldd1%2B4fAtp07g%2BSuCmFQvJpBcQE%2BXOIXZqzjVc9xUMe2kmwoaD1KnCOoySyRGHPfgjftJyNEBfvJXbs1RDxmxpDshFAdvOh49QTZoOAcMDcPsj27ZZQurhTK%2FqC7v4QT%2FsLJ18R0ScTEElP%2BYze019lWnikYHdE%2Bv%2F%2B4%2F1VXeTY84PAwH0xpH8a0%2Bt7H5OKT40gUw%2Fr7Sggu6EqGbJ8&X-Amz-Signature=85faf548f602ce8beb479509fbb86146748dec5e09c20dadb2b2eb92b144f9f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
