---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R4Y7JB67%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T140041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHh38yCvbAv6%2B9TRVj7YMrxIJMvWfHHGQXhtLZ5El8VwIhAO8uLTb1g35Ei34r1flZTqA6CJ7pbBzN50N64c821a7%2BKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyGrTokqQMSl%2FkL3lsq3AOfwbMFMc3XQt2uX9CfA4Q8TY1lTmW1odeY8orjLO%2FCyO4LUMrkGvlJM%2Bhfyi5vpnRO2Hpj2zvz8E9oliOPKG0GlQxRSk4SiYwuZJDVTgECgqHH88KI48Va4JJ869REuF8xXXzEDhQvdhgqqW9oqxGjO0WVEL5xM5BlSm7OYUP7J9vSV4nG7ucJPFQSmDCBowBCi5jRd1v1LFx4kTjlpMejGoTNPg34vYZcPcRiqapF2%2FkMnvLEwN0%2FFSB0PzaUi6XT4Uudh%2FwOMrVxS32WM5avfisg%2B4L4r31gIvXTiEsHH0g9P9W1%2FKHjoaUHngMuAgqZhHdMUqQiaXbKBW9xglaTfvB2ErReHS9vXP%2BzKeuI63XPUw7GKDuIpAx9OWNuKoEmrhqOi77tzjgOt58JYhvt8b38eCFzHgmvPmJE3p5RcWfRF65H5DxlUzvipfRvj2%2FbGlL4k6Js0amJPtJPs0iDURWoeCXTWEFD4ivEtvPOjJXD3TLK6UOS87YiVmuaNEloD5EQi04VRo55wjzMfDVecs2NLsee%2BCUoZY4kYQWyea0yEnm8J8aoJ75TG6hebQvu7URy8SCLl51bnaxV7zEMKLxv%2Be94eR2PlMcA8GO5HrXIH7qKO793NejSBDCPwKbJBjqkAQ3kUd6w1piW%2FGCJdUHxozj6m8hABDQz0NJ%2FU1EkI25KEvl56ogWK6mdi1Yo4avsN53gR4Owxl4g2zkcZnc0ZdcAK5dlx8yyxnsJLbVIVbx0dEi%2BVj5Sqj8zjqPMw8zjSrzQWPtAoa7hQB%2Fx6szSJU9cRVWjt3XFWGXtxBy%2FToxvTFfGlYm1ry22kH73GmcPHgG186hTu%2FTmWGykdGB2jaNQFapn&X-Amz-Signature=605721fe1bb604e980cd03d5aa6c49ab3e97808972f1d3c0ad431174acd84da4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
