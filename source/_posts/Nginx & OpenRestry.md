---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXIUWUXB%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQDKvJnH%2Bxtsv%2FVyiw4c%2FXzoLZ1PgpBRiqAvBV29oSGb7QIhAOvk9MSOvB2qTm5imSQBuQUpnGDVfJRjfYXdVeTcaWEAKv8DCCMQABoMNjM3NDIzMTgzODA1IgwYAwzNLIR53elahNQq3ANyXAgS%2Bkx%2FjfZSqoelsihgQNb05b%2BRYbwnlPzK5BUeA0foAmEOULUeLoUSHuRfcwu%2BMJl0GjyAOJm20QJ%2BdA1ytCWcCyMod1IBoi0hZDuSD1a0stFpd5HHtvTden%2BYeARyMuxl1ldEaxZGMiYkp5KItAdZ9tC%2B0aoNrWwpnZxJlz5qjVKacfZ9v1GqabItqc%2B7ce3fIqmEe7e68N7Ei7ePeLkty0LMlfiv%2Flvx9aSIbu%2FJmH5t95dcod9rvMl2BpBS5N3Se%2BgHmy4dbR22XaW9VxNorm%2BMfPKKFEBNGO55RCGE5O46dAgX%2Bj%2FhkU6iCx6SlmfCpljVl13RW3Eu%2FyeEExALikSvtMFk0sAbnnpf9rZ6nH%2F1OjZ9JS%2BgltBYjaT9TNlw2VVeKZhrV9emLFzGRwsP9xYjmERYqFDJgyLFgI1PinBjPuj4eAARfMRCFMBScs708XFIavvPr4JDJnAUbo76WH3QappRcUgK%2FNmMp5JRM%2FCqV3G5tJMra6wzI5XV2ipIT7w1QznGfBuSW1bmtrBuGXFzU6xrhuGRYUVE%2F8qy%2BCYxPPWzdRLvFIp8KVUtN0cbvhpTf31r08thmntn9%2FbxewRwFk4p%2BNcF9oJ1dM%2BYC7cjOiPGFwhhUjDixZXIBjqkAVs5o0XqLCzcJrxcPLdp%2BbCC3tl3jcSWbOUMj2xOxlOc7%2F9szEEE6K9POl%2F93MRLB%2FBMIfbLSYs2%2BC3gJSOFS%2Bs2X2bmnIepG2F6mjNJ%2BUrzdt31blHT3iavt%2BSdElIgheAhwz0sp3Yn3tTFTyM5gVmErVDrGGnUUcYzTP4GA%2Bs410ib1Qd0c8J9YTjdLeQC5f4%2BfhcgctY%2BCCdvq3bx9I3cGpAc&X-Amz-Signature=a1319ec0552789b11f9a12b593635d1ea05eab87382f5a5e89dc52c06f08186e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
