---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDUVIP4%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID7bhHLPeh4W8XO02GXuhhlkHihxISYhblblvjNT1mKDAiAWz8byUFajbe3fp5szNOYzkaQr1pIDjDNOEyTvQKbvlSqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqwf3E5qGjyjBr6m2KtwD1LkDUmXGraNZF4uBbxV4w9z0nVXd8W4BoaJ4ZRXGVMDMy7P6S90EEMea8XaQ%2FA2LVDaFc5qXJi6ovMD5YSFPLFxUejD3y%2FlYqgjdbH8SFRyc6Rz6SwfCWqzzw35owSeCRUT6s%2FiTzdRxCXob9egvKPZGhkSaagGd5RDfYafUo4xxM3GBJzW7cVj61fO8%2BxDRhDdWa%2B541OfENiIKdt7vbtkhJ%2F3gDBC%2FkTrOCOa0q5VueLCtBPtLuT571%2FaNg3Bq2GDFSCDtcmRLcuhceFjpWC%2B2w%2BQEkQJpCxcuBZ1r19mh2%2BNx5xpsOydXUzbHwP54H3OeeNQSqL%2F6U1C9mPFrHKL6bxz4DHZ1FE9cBKIWwiGOoy%2BRwBmD%2FXI4Wn%2FHavwXXgg7abEd0B455%2F97M2k6Re2HlY19N9vlkP2oYx66rBrhBFFnK7xPnQZVBmi9ANee09SymCSGy28R2xae02UPPUpFxqeBKFktJwDvWtyiXiC8K5syCl3nsE%2FQD2Chmn52Fr13G1emYs3cyMkvKFermA7HCeIM0O9QajB2T8y2K%2Fbb6%2F2IuC805KGvixP5hj5CkcKR%2FyeMnUlbvfJiHfd%2Fd%2FLvPcz418VBwERCyUawON6pEPjBtQP7tYBPyngw2dnCxwY6pgFyCpDF%2FNURB%2BS3Ogqude%2BUgE6eMMn0pCfSPY2mXyP7ISW3RpWF9evMuPtmIwzRbkbsB7J4RiIrvJH0yKsvkLDwv4nyiKP9TVbp23eDPkDBc8lG9KptZYqyAPq%2BnP%2F%2BTFMy4tYn%2FhAxQW0aBufGXH5TMzRfyyFdnOS6w3d59cfKHN1JDz3Fa4tts0rcKNgNTt022M9jg%2FoTCfeNKKS%2BhlZVW31zRaW5&X-Amz-Signature=8985c285e31ed3c3b0e04e44faa274f7497f4c856225e756716ad8c29008e09c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
