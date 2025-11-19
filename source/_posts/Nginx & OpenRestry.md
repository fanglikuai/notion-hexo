---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYSBUIHK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCmqfs30L%2B1OzyNXN7a5UfSvBQlh81u8%2FrPoOe3CSx2swIgDWaUcsH8cQUCA5wntR%2BNaa6aWY4W75DdOOFfNqppyWYqiAQI1f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIeN2naGipNFPWfw1SrcA4jH3r3GEdxGm7z7wi5QeoyKVGVfWjIZv0KpEJCfJw7IfDvuUoyah5dwrS%2BjU1gfCWXjJd89Nk30vSsNcl%2FdRKIGajDRU8GFnB09A2mK4xRK9dEzGq8Skgt42d3nP7pkHE%2Fedx0tC6AcGnjiu2NpoQMKGnulyTp5BzAAkE%2Fj84HYUg9WapY0KVrSeJCVUYQAECzSomAGOP%2BXpAJPxLmPMBCNF8jDTZLdeKiDIwihfNkT35iMbm%2FGRieeSZBJa3S7HENAkXSQyisEcbHPTxZyvaO8v3FrvGiAaYsdPe2picOmrGEjV72rjlC2hndLN09WnyU8gXYiulA1Pyden7KjzYW1dAc9VzsrsY3M9vDk8%2B3v%2BFZARQ4cEKF2rFx3AieDIKHb6ejO2RgwhW4Rv3Eb4IzhzQp1aDxp%2B4b5mrw%2BlO2NxVsGbIJE5HGtWsMhuHvobibKW9%2BNjFG9op2qC%2BOOmVkRI96q7R%2Fpn82sFH%2BS3IaMU%2F5V%2Fugs4%2F0tb8uIQZmCpWTBArg4w8j6GgT2%2FjHddVYMpOADwdRiUg59R42Qc51yw8F5gNA5SGGwlfUmzdAuKeWlbGOudOqbRqsIpP4vlo4rxakMyEd0IA3un4BIVuw6J1kxavCBzCGkBpj9MM%2F29MgGOqUB0EAzaeSomyZWq1USelx494I22PxCsuXr5U%2BwAScahfuakWRp7ilncyt1GfYOPmC7kY9jhKUDUgrqugU3%2BNeIwPx1av5sfPz%2BsBGZIblp4k9Y9zzpaaMaldsDpCinIwgreNU6w8M%2BRMEyrovgj6cs5xmTSwSAi%2FPlPEtU6IsHyUC0qgueWtp8mEWUxkuhSmFIod1BftG1UsoHn9gh0X7Btzhh5noY&X-Amz-Signature=6802941c1c1a5af75f79d97f4e1730a2ca6cb6ce23a4b2cb699d3ca51b91cf98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
