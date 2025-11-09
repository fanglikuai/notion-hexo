---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653T2LTAU%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIGJwCzzX7bK2lbPExOEgdOtWgXiPD2t4kKRR1ECaps%2FfAiEAr%2BEp9veYktxabdkt56MHsdvnb7kCO4IxXrf6LhXNQGsqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBSGIsfzVL6%2F%2B1DVSyrcA9gc8V7voJ2Z8nE1MmQI%2FKr2us5pvURxqmRHBGDNlUuB3RPnNgRO%2FZXeQKduCMyQNtHFCjYalBPs02R3tuuDNIzwHOcqgGnLUUvwhm3h23EOJsmD3YirrEPMRMhb1MspPvR4uOx7KZ6iDFNhtqwNUfgFZpibw3KVQxrY6D2Tu%2B6Lg6RMb02011oPEXNsrYoGMZeB6g7Qo1L9BTJtMQYWk0Np2XEtkxE4aeXfN1ZGXiQPsahtlPe7M8kQGHMueSzRz8WtO7JxxWrG%2FRzvVC420J1A6tf%2Finf86E38CXiiaKNTWUhgB8em2j8b%2FUUtoYfuu755At7anJixjNaq63dbkVbSIVCCSsf5YedOwZwqmGUgrrS9mQXuB8f6Ocqjbf8IPlDLnUk7OUACfAUWR6ljGCQgiCNb%2BX1dfEw2hhxziixX7X2ndJh9dw%2ByVxYn%2FouC1DWVLEYlsiQAhRqUYyEdFu1J3phFNChZMlksbvuMC%2BwxlO1oKGc3q8vx2Rjk5diT4JI%2BheyHO%2BqBojhqj0mSKRmCH1BWNsXOV2dej3k4%2B%2FAIigSOeUdo4%2F2IHegeR2xpuBDFFw0P93L%2BkYmSZn3lpbT79KERJL56PUbkPSzcYCurfHO8YaZMDEjiDA%2F5MO%2Bmv8gGOqUBg0ZEgmI3KmlTu4PBUetUU9ajhmPluPUFIcEe1olyeb4Z1jvGYPJXNr6qiDuGSJ7xhVSSNplX2Qa7USVEc%2FLNyW72NB8jK63DcSTWzCcP2qzHC0nKJYPk4fReXRvlm04WDR%2Fgfxj%2FpPCyCf7PDJ7LpTsC35zgVsGPogBBafTTsfAHy0oZijHQrGUoXAtlnyNNetkaOLMKVaXA0Bc8LUuX3Tfk8nPy&X-Amz-Signature=c01907e192bc38ced1a09358d6ecc12c5a57f6f5790f6e4a854a2cf5068c1697&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
