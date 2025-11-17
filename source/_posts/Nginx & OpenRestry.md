---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q6OTXMAJ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICadG%2F608L3KbG18FDEkYam4Mo8k%2Fq8Hbrj1btAj1CCvAiAoU51IlpNaN00y5lZ%2BLDFxif5iUn30Hs8bS7xAwrU1RCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPZ9kV0GCKVAhyOKCKtwD4v9dFCcwS%2FbDVRyQ%2B%2FJKlhl7T3em9khMACSk8gYRvTqpJAXJC245SnFgrXYB%2B8gmIACkyJ6hnOVvuOmoT7jjdMXkTh5dJIO87rC1vPtKpmSqd3tk%2BDjyB6x4FULARvsDGPV5CagRnx4Vsu4qwTqIzscYl08Xd9mm7BtpFIqDiCiMUg%2Ft%2Fcgs52GRe%2FOfEZwqQ2HVGez4SSb7z8Z1jYhJpFZWKV%2BsK8ZbMP0hjYG42Shj3zh5cp%2F5fBDwA0qRK%2FVKfonXccUerG%2BbU48x6ihedGyykEyw4Do010gO0zOUR8u9a%2FlzEhtvoAz%2B2QgttLIG1G3NsBCBpJqwC0ZZ9qSA57UWJbvZBJD1H1lqj9yB4um8um%2B5PHES0inxnpGn%2F3pTcQOuobLq0GzzLRZ3%2FfLUlUz01rXjy9%2F6Wx9ItDojI%2Fm9XwiEckwaHiMwLDYdlkUyIMS43kbrXm5hsEcBiWmzCFgqG%2B%2ByvIwOCMHlt5nnitPrEOy87c%2F9B67QJF5ajMBUXYJsIbRQA7M9%2FaB2MDtQnGOE6sYnFQ35evZtBMOl91V5ptEL9gf50Izo3HAxPmp7RhmpDX9vqNElAfATuMGiX%2FOfsOgT1mRWuqzzQT6rmcsVG3yMpYdxbrPyyF0w58XtyAY6pgF2beD5eI3gOeL10N8ALzC7evqaCi4eNfxiKyjqz6qyELbBCxbnfrUPBK45qKRmees5kj33L%2F2ZSZ4euQSRR86ONANQNtzibm%2BBzRodDShN5SJYu%2FZoBTIkbVmtz2%2BY4BOICBW2q0KCYEud4uPXxN8LS3Jv2e2S3hWBtO8jROorevseP9rtNXu9kl7uSgQ0naJnkR0%2FM5IavYIXBODK9QLj%2FRPOh3TY&X-Amz-Signature=18355b5d36755a64513f6866b05d010fb08543a3a9c88745d40a9724653e4f3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
