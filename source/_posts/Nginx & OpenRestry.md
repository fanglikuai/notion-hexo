---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665STVDGUS%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQDl0TP2WRt87UTFmdQZUNPdSNMFiZjWP9XBLlYxIpcxvgIgOCEOWjNbbO0srxI2TzoW6ecLYbbil3gfWhtIFEE20a0qiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBMtQxSz4ZyR2u%2F%2FmCrcA%2BaHjBFMuHo3AHsjVGPvf%2FlZxRF6JunZwwVxOXPIYhTcDZgoMTi1DWVscl5fTgStlvhHShJKbvpL%2B6U9Y4R9%2Fn4cKr6bmIeJw3SA4S1I2LL%2BvBQlj5GwJv0Nf%2BgPMeewHxxh8j%2FVtqp1BTR%2F2yn7zTv%2Ff2VQk9lV0FWBBQSzFjfK0T9jsSmLfJe1KoqrLxPsR45ajFd7zuJe6cXEgzCcSRUHyQ0KSAI3K8Fy5bzOMs8HcPBIYI7kaOsWu9zVOiq2avcoQsKiBd03%2FHuw5Yk8HRYwi2UCmqEUzhDHNY4rSaP7yxU7%2FVTTy2UD6nvSXTTsAkrtN8WfDVNQI6b4znMb%2Fa2D3RZJENkYWVyq7MyLp47tqu7BJAc4ONheJ793teTntqdz5AJA7k5djcGVdmobA5%2B1y8XEloxyyrNQ%2FoR0Zkl9wtI4t6zHNkRIsQ7aV4TAlrA5xN99fmceKW2GXe4JhcpvsQwQMAfm9b4JnWHfIfzdyleQaOQ%2FC6Y2VHMVug%2FHlqAXzsugxE5ONgvyq6CeMJzdxHgMz8TrnvRzGWW3izHvmFCmBtOtXZD5bribHpuN06Qhk4TQlccq6gFkbycJ1TNW2P%2B4rvQf2dgtiXuf%2F3UCHPkKl73ADewdJzOMMMW%2F7sYGOqUBpfnSTt8xPRzf2U7catW6xacb%2BkLFSmvzCCO5wPtmJ7mJBEndq0TgRg8qtd757Vnk8L37ujnqSHSf667HPkRU0odHRgTc7H36%2Bt8bIz0oRujdkYqj79bdLuWEyH3aACHOX5jgCbr6KQB%2BbAsJg4MpFOfCbFiIkTWFC347D%2BKY8AP9XgJZqgmuIAXyTrggNjAhM0HzBtamnA5SH%2F8AX8EkIR5Y8FQq&X-Amz-Signature=fb34a5e79a991236a8e373a0388ebd3af0d669f0d0e0bb7f540e0803af06530b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
