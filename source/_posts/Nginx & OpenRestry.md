---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC5UAMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzmv99YQRukgJVJtJePeGARe1fv0v%2BpGZKt%2F0WoVKT6AiBrQAjZifJTDB1SgWVhl7LD3b2T2UwZ3hAt6BVE1u8uhCr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjmek9NcEb8CsWV19KtwD2cIptCmM1iATcu8Oq7A2VR7UKsiFADptrRnufCvkdMylbKLiLF4ChL0Hui8KgO77I%2BbuJX0edspxImbG6eL7HxsSF0dSaBe5xM3zn1KWTfZad%2Bv8bBXK6qIzNcCKowvDuFTrYTlzyZ5jVnPaaqB1bqX89NabvO37Ka%2F9KBhdUz2KRyyFitmPhT5kTf3ZJwpFXKlYo3K43yx4i%2B0uv52lAxMBQXivkM%2FetD3tE5vXvsUpsK8R5g4KYGPiFybvLGaWT0bQTtHP538KgED7BsuyMtByGyH8cB5tlIdsAd3HB9epnVA8nQ%2FtjbRQOhqfonJ%2BgVkFWMj2CRuSJ7W%2FsW3FIxVcwd7Zha7GsH8gwpxy7xj929Ge1MoLaX33Rejcehr3WXcRHYnEXFJnRALmDPnX3TvBkpterO%2Fu3BHvKs48mlRzf9LT%2BdItBas761L06BVq%2Bvbr7sqTjn4ZlfgbSNZpD4dub%2Blv8itVJnqH0ETG7yGC5FKus9DYykRYCt%2B0cyMdeTY7W2Vnr8vcXA%2BKKkxisQ2JYns%2BZ2iltbtfTUDdmJ2FwVnjMUg0wksQs1xCE286q2d7Fe6T%2F%2B9%2F8sVvJTJFe5ku2DRfq%2BJOZVEdpGqBIRzsAfjenxnqmuDBydAwnaS%2FxgY6pgGzcL8dCJ53NuDUT%2FAQTIBrBJpc%2BK992Bb2aecLzfLIRtHSq%2B9tWG1BxIFVuhvZF9fuCaVb8hXNAFfj8Ypsfs791z9VcyiDy2F2vHtuCvJYctH0sLGMzmdjDzHxAjXaAtf%2Fik%2F%2FNG97AczoYXniMhwm77fwvINVG1btYrkmk88c1eX8ZNynGJgjUDUnoZHK9teMQMh7AYzu%2BXBBByP40HZfqW6SiaHM&X-Amz-Signature=8e5087abd05c11099c0c8df05b96e1ee72464835a5e82d008cd9efb659c9499a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
