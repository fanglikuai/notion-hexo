---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ULHHILK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQC3NLLIX17S9kj129iUkATWdMw8yH%2B1h7ZhAs%2F3Kt%2BJbgIgcTUJqBGZXZIKIIjh0v%2FR76a6vMVP%2FMOPH5Ho129N1nwqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN95T42TwI7mpntV%2FyrcAyzz55Qs7d8T2MKFPDE%2FtuT1zR6oZvDpTfPBWRKNPejlBzaKiFi2hugsVO3iK6P%2FR7%2FNW4gjNdyiYK0TG403b48GylSJUabUHKH%2FBjk%2FhSbytQNiIXj%2BxFahTzWCITMyBhFCsZ00LbT78dA8%2BKcLWKXkA3V88KzY0zzTk3Tan43nJOn8uK%2BNbwU6v0vCQ357u5cEEPd0wHA17%2B%2BgEEi%2FWKD79Y5NAFo8JzM3SIDVjaq3VU4Wh7Tjsir5CYr5kSQOfGyoTOdwHcmVn049t0XNPp5gYSTAWLNiaylbSXuCCa49bcYc9hVZ1lNa6qukWWSPY%2BaJNjOCR6EZa1yO8u0m3P15IEf%2BSTa1mebQcMOdWkvVu%2Frmud2Y63nJFdqJsN1XOXWRgHR2J0G9ifr0cHFBQjeGIf%2FO6nfOn1khO2FTQr6gG3PXglGI6Go6Kh46qv9u6wkwJ945gSa3ugg%2Bkt6oU9g%2FzUVn%2Fii5WYnnh2QxzSrW1p2AIf%2FK4xlQ2a6zg%2F%2BXul824hs7rsKMX%2BPDf%2BWl%2BedhRYptYW8dV1rnZdTP0MF45k3z9a%2BQMNJ23V%2FjhgXQawWvjA1GXVVD%2FMRjM3ysK0tIjZzbZeEs2B3DUvA8JUv9ld3UwoxgX6tjkzxQMJfqnccGOqUBERHo6fYa%2BUTOqvOUc0UqWHkerl1002tK8a8VuXa4q20KY1CgMtpPX8%2BVzGfbLZ%2B21EA7sHTSOqBA2fkQ6I2vZoJ0d8bK8%2FuewNEB895vCW9i%2BEYyGsKAQ5p9tMM8mEQsYrcM1haIKE37PuY%2FsrFpf7IB2qozCSo8rtkcT4a7Ok6b1kfXuf922ExcAiRp%2BD1dRd4dsU7Tth%2FWHlMoqRar%2B5md6I6V&X-Amz-Signature=c1843d4b64c38b58b72de7c584c6d768bd9884c8416b6feb514cb952f665450c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
