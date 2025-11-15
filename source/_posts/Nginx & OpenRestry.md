---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLX2MVEX%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDyJfcGqJKZGfx9pdORbmDKjps3Crsb36FDwIZRqFgnaAIgOsLGPPknhkKIfQXuSTyMpZvr5LgcXMm4DVT2uqCrxl8q%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDJQQNw6yxa3EARKAISrcA70XMfmOXoOdeH1D54RxPOmeRwCHD6VE7WojGHcGHfetkwhAOzlvgM3c2SWaYTx9w5VT7jrb4llpHmFyNK5nOGCjAECFdhWvXQBYRWJlbJBxJfoZQOSoMTO8bRB62WbseVKoy3CA5ZeM6NfOlQD0lm%2B%2FV%2B9LL7U7uUufJ%2FOgSKl%2FOc5EgRIQ1WP5Yxj9tIyDATs44gC6c0Bt9V%2Fm1YgLTwZ9fKdFQXPatjoj5cr%2FQmEIGIkcBUBhloGRM%2BOBj6QPwNd3KwlrO0DRLrC79M78QH13K6p9mOfQL9bGOkCj9TlHN9qWTZ8eVDLyiiuSKxoHRqdIauRQA6a7DWMtTYMcddS2dxIJCqJaBK76VulRPwsYtcMpfQqOYMA1baDcKCD4zScs78y7Y1i9X50g5QS0pNmr3%2F2UmcabTEUbuCkHaeF1gBxPkbxYCUknwgYC%2Ftjwzu5fCZ94pWU6XSy7HMwhbBf4wDRt2250g5ZQgfrZQaODhvOtcXg83SSrEsk%2Bbh9YcBuJPDWUj%2BC0%2BXSSrR%2F6aEDEZnrtJdsLNDgmRF6HGsKxGsXmmmF08kZ5qWWCGfyz2Tmu8WcfbbEp2TPBO69jqV2Zaum801VfKi0drpSwAFWVPkGsUKt6f9iFZ1WgMJOi4MgGOqUBAuCEj%2BonPcq72o5e4RfwQYxvgVhT%2BrXghJcD06kqk4qwrloSDTgO1qBvEZxVpvnqyDdYNy2%2BX0n4Yw8sIj2a22rS3A1YUVjk62gMsdrMOwTqy2VOrhAJXhz%2B%2FQiWHuhd991nkEmsaqUZdV3ZloQDVMutfmCBlCG57YV7336Yo0ab%2BlJcEQJqgv3p6GYFPxLolJd0CF3ZBZnbGZkGSoVhGo1csiQY&X-Amz-Signature=b3b75c6bcac5c3a7add3bd85f8e37c8200bc0f90c16a2d4886fdacfd14bf5402&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
