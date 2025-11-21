---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665X5ZEFOW%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCIH1UeyrFZmrMBHpOn3lH%2Fciu%2FDLRR%2BdbbfTDXsvlu2zpAiA%2F1ESldAtDnXl1EE9bvJ3Ib89pJSM0rU%2BWKHiB1xslsSr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMusTc1aw%2BI4ld7HrbKtwDuSqo2ijk5NVRa3J0s6COYL0CuUlYH63q%2BCGlTsWKnoyRqFfMF1Eg5cr9yXTieGcVk5CzktG3tHD3X6%2FKZV1E4Ed2mC%2F9nlEyXvrL4QcFBagQzhmIiw1KUbYgV%2F%2Bp7HQAuzck2oazg1tuYAiqXI59GDK3zL1iHIS1vNioio2V%2FDRvJIXRyYGDKeDHjsMJwUzjtEX0ktMsgixbZZ5Slurop9vdosuoGx7mngzHT30ASzkUfERonJspEkQuj6ZScEcGVCCsnxArQk2UUcD%2F6vBCJnLfwx37Ca36pcVBZuxksC4%2Ba%2FFJSJ4NWqo%2ByWaffml7ZfdHfAPnIF1x1AfBwcZAA2neKMfjuMU10TQQC4WoZ11gTsfd3NXT6I5xstSKEUxuhM8G23NQW4Ot1tgBpY9xtAUv1A5EBnq8gvTzhAuJ32KCiS3eW%2FVbrMGmEChAte7wmRYFP3cgQagjrbXTtRTeU71gAbCml4RfITLtZGmzXpsg9meADsvDgUbu2JMmjnMVu23kpR9TQ0Kg0X3nXAud9hMhKxcCyewHKw9ZV1ttOLxZxD44%2Fh63NCLTQpewCfo%2FHBKNYLdMrAR4L3z80sNoY5VgIWmfi5gfazWxH5n5%2FOohJiuMOQPk%2F5WfhhkwuIiDyQY6pgHY%2B6RITdnKl%2FSqZTxatNCXHcgOl0qIuKZqdxRarO0Uo9NQs7uqeMmxmEM3M2JEC%2FqMLNkvQgYLSCMRYS0MPhGHv82Sk6Gw2rkVV7%2Fep%2B%2BRePlokra0Tfi0shJY84jdVINrqM%2BnCte1t6v8h%2FxMKWDNi%2FyBjfvV9RCB8zb1LCwUjVxDRaislDoPKIizdSUq%2BWR%2BJRdQc11N65LEUymXtVOkI%2BgCxxJn&X-Amz-Signature=940672904d2d97f45a5154d470e54e8b69158303cc1e77be94cea34ecb7d29cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
