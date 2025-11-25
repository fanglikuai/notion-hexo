---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643UVWU2D%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCV2W6HlFTt%2Bp7iFFG2lw21KXh1Bfe3dqTaG3%2FHY%2FdoAAIgE0TrxBimeSmGtp1Wocum3x%2BPPdHFROItCo8hKlsGU%2Fcq%2FwMIZBAAGgw2Mzc0MjMxODM4MDUiDLCsBMskkvxUDjnB%2BircAzgVpJIlxkv3%2BRnqNMCtDRC7sUBF4tI6JdruCVwyZXrLjmyNDY%2BfvFl2pzuP1109re9yp9K2k9hfxfZmbryRBPt8u8gDm1qMJUbfREX2Xm%2BOKSiUoe1KDeFW1XTVotaWgND%2B4JSjn1O%2BZVWNH7viV0dCvkeVnFdExZj0pwMGiv62QKrbQhnOFqyPekPGfgky4xZYjAAqKoMHHNKIbPhUHzPRmjijdA9oriWdhJnVhyiqkKUoGiTOeoOGOk8Ktw4CmGmoDZpCEaG3yIkduqw4yzBABLWHmFZwvhb2vgTGQoA8OEWTJIUVmrFz5vqEm37qt2gRwLo%2B87t242jkg6pMBNZi20A3Mb3zgK0Gwpiri5Rn1KI4pNGDzfhooPd4mQuDtmhUISg4ed6v0johJcN42UWUe%2B2viL9K5Y877xErdLPp3i5UBrGVMER4rjHZlmU%2BHA90sAK2UhbHEZUbh7VGd%2FTqmbT4XTPt4r1vJFOf74zlc0n0u5LMASlWbFTWV5ZnQQnZuiI2IJ9hhjhxVJctnPwuqqf%2FvWVMeRKbWWqlaiDra9fJTsObDz%2ByXZwAil2a40wUXtM6tzVB%2BgE2Y4x1c0KG%2FdECPBN%2FEvnFGsE7KMOjGh0Ez5ZfWoTuGkX4MKS5lMkGOqUBZYDYoarproLna%2Bi%2FVbIFTiEyEi7ENvHMIfelUo%2BhdGsNduAhdlYvioLyei%2B3s%2Fe2LYgWthTJpRMUvqehRKm4JFKa0%2F9JUzxYfLdpUc00fuwBNY6Ij%2BKYa3SEkCqP%2BPzv4LTFYbCxjscOqyHr0SRa1pIUm8GZkouws%2BtrHG%2B5xEJv41iYUkC4J8pZcwF7lKvDeYKYUGhDGRjIRIb9yuF4K7rDKCAj&X-Amz-Signature=158f3ee137ef4030a59007e97d4847e2cba7230ab9be3326bfa76ccc871f870b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
