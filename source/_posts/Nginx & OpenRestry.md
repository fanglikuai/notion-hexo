---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGEQKXQ4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T040055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD1l8ukE4UhtSZHqPGy7qySa60IzYdrhMFTNIhBgGbiUgIgOo2GHR322DYZrKCWmTKyByfxpgjUB2yf4hGDM35%2Bffsq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDHSDFMLZS7ikYv0UpircA3X9cT0ibd7zvj190C11Y28Any8Vw55klJp3AuCeN0tRv9Aqh2FUa9vgJKPENfkzJ4hG81ydmWZMxZIRmeIy3d7X5F0GvmpG%2FCeRYPS67IESSqZ29PdLieAm8%2FWCY%2FqHm6BfdSkwtEA%2BKt9nY4IlkxchnCaqwDDu8RI2lnAfL0H5mrWt1KVn6Re918nruDzm4iga%2F8LOkkh3Rs2PvHrUt%2BmPdD8NbMrGh%2BH%2FqPMCSlTF4L%2BmBmGHTGqYAIt%2BFJ%2FyCUcceVRvVckD69DmtHDxAoqWQmnjdLGg6D03erc%2F1fH0gm8elB5uLsylMRuHFY%2F18%2FXNiVYRQUgEH6aoKg5HgT27c7NoiLvNh8pb6UoB6jVoFLPar2veews%2FxpBgjM1SMCI0vswckpNMpkCLcNbBN2LJMCKz3qtXv0Gi3dqQ9p%2BqF%2Bdp0IPzrdAbUvnhAXKBvARIJ%2BqyioOr6BTmGMDWvJJzrlF4QaHNbzRvScawIH0b3ZWqoZuZ4p8smKgJfs9uYka20FueIvawLj6nGGbbbQt3rKMZxyHthf93J70AYPMZaiGFOWVKCmG6W9yJkOQmZ9%2FLLGEmOAfB8bGN6974pasc7O25dCvI4lvn4EvHk0MXNC6RwRFFohvT7oFrMNuM8ccGOqUBd9ordSynTgiY1YVkWd1CjlqGTC%2Fj6nrxEEUAwl5p37d%2B1SPqHW3uvp0mwWKiu15sjcqPD%2B3B8W12iqlork3WIo5Dcf21l0ExVOGeUvYABO0BNr5IddeXM8sH8u3RX6%2Fvz1CBsh%2FO%2B7IXNwP2sUgE%2BcEGSiJbH6D7QXGeEvJ2F23almrhIQW7feucJhLV06Lz1G4PSYzib9WZA5aplVl5k7Om5OoJ&X-Amz-Signature=6f290c288d1272b4f3cce6b6a0b92a8aab347622b09ace1577f57407774d0e2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
