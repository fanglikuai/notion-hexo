---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IUX2K6E%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIFA4PuHoDpYg76SE6qdX8wXmKJEJhrgZWgNwrSAQd%2FlVAiBXNBHq5HbLv7KM6JNHGz6H3rMeebOGJej%2BFsSZpMCk2CqIBAib%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTtfGFokEju9vrnT5KtwD2EDLy9aMX93HcOGbKju9UphXMsV54xMOLwBQX%2FMZPa6WnFrJac9fvGcljdsK9H08NE%2BHEvJFr%2F2UQZVbFIE6sWm%2Fzyn0m4sDllWl5RFFNOnwJW5E4pPiCT1OKT3UUm%2BXAizwWuzN%2Bwia94adLBpgih4UjHolBE26AMBmKnVyAOXIXJNgh95nziokBh7lPOn%2FypzYLY6Eq0ffC9xgC7UQZjaABtw9cDrqKIkoFvCZrRS%2FjUuZvoPZtmb0A3E6QJjKH0WKICQgMluaSIWeYpsoeDO%2Fbx46Oe%2FXqNfDpSRYWVG2WJHl2ydKK5CC%2BkNkGSdXchjXgChWM%2BNyD66tZJn0P3co5S%2FQDNuaZImFh3uu1%2BS1Rk0sSqTmhI%2BoOPHKNQ1vcgSUAdOz11%2B4K3%2FdRnLuGcM7oA9h1fEOGigm94PpaOavyGu%2BWpVD%2F6Z9G61Hsa1LoI8a%2FWS0xxJULRmPLUp3ln%2BllLOurB99RjewA1WomIrup7RnwMkLUKtAzs3GWZGTL7kfJG79rJ37GOtINeUr1VyaWP6KGl5zCMIcjQ9TdIYLkk3hghfaA2eC%2FFAR2f5zHzW4LP%2BPFSp%2BE9bp1uUbrREe9c%2FFPqMehkFStrjFUFissBVW1YqA91V%2BVmUw2rSoxgY6pgEBcPD2PZ%2BHWosQyDH7qOJNotXDhxiWSM9V%2B4oQm6bwQZw%2FqGGj9spMnc%2FLzzzLekNhRZMAycDImibJNH%2FUvxNrrX9DbCptfOmQVehvalbAEJERoeGZJlGyib7iPsUTz1DpNNm55sjvDTXf7VEa0FD8UKFqFoREJvmquVtGaD5oBPXhkpfKtcbSkw2TGJDOxajy0Tbtcv1u%2FZvvAYBRyMfj4CUD7oJT&X-Amz-Signature=c82c6637015726dec57b204b4fe91dfd2c85529f6bd1b105a2669d9b36a6c5b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
