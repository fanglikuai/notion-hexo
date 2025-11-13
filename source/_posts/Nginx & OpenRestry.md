---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3YCIV4%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNDJBrtmWB25SrvmM0rV8sv2R8%2Bn51J%2FkFEnchHVie9QIhAIqjZQvI4xrXVtjXIkngwN7Gdfx9OurQCXvSmZCaD1A0Kv8DCEoQABoMNjM3NDIzMTgzODA1IgzBL46mgqbaO5SIvyoq3AOIK%2FjF0OhHzjrlYPOJC9ebNrBbgQBtJWmaqg7Z%2FbbnDRS8FtDk2pIvBgmt0FcWzSOn266FgR9m7WVmHQeBmK3c78rFNPY%2FiMzFAvTVCabHDDpnYUJ5cZBUI2ANp2a%2BTg5B9UkYtLLKC7rv60HbMOWo3Veq3VEEW63Z7DFGnJm2G8EvpRgm3TehWiaI6GWID41XadZFFcEL1oJvGIBlLaMalp6RkWpTBlpVNWH%2FDV4WTwEEHjr%2Bz5JQaYzNXaSClDM0TMiyNMlN3OLE2ha9GrSspCp5pPKZoiKzQ3JHdW6McYFacksyFx5Tmbo068PQRird3trUIh3dPoQ%2FHIvHBp9xSHWBNQuULyPaWhuB0MrpiLxRuiWaUu56%2FDdzmAnPsU1Vk93rjBt0GRFyAZY%2BdsdNwxANj2oI%2FsjtOJxjDuDWbb6vWy9uWyMZziW5yabXowJfODBlC3mFnyTMBTkSRP1iODWGunbmma7i2Y7pylCo0n%2B46%2B0TOGmPBXXH%2F6yr4VsCOA7Os%2BgasnZnISMpFD46AfJvJAnneRtRmJSfM19AJc6hk45R8YYBG%2FTLZzmKHI9I5olo%2FbvC3GRrS%2FJ3LY48pqPBc9uwYVIrgWiEk8A0A3P%2BFkIuDyRW1%2FmW6jC3w9bIBjqkAUltBoLtxwZcw6RR%2BcoXAbaHfmlaQqm6v%2BQ8sglAuGuVcY8IKfc1AtoHuRtnQM7DmtK7DlLvz%2BAj6heQCa1P9RMOu8QL9KHI9ly6G4eGx0uf7Y5LIG4s1D6z%2Bjde%2BUQcQo2lAE5r4%2Flu5gxaUdbMs1Ju%2FoHa3j%2F3fUKvXOYfmrMfunO%2BqJL%2BqDkJiDrYf5HIdjiUrmKw%2FNAD9ljE1NKzIFaRaM3H&X-Amz-Signature=3a79d70620eef886f0c47387495ebf24f71fb2a785b94132581362951db974ff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
