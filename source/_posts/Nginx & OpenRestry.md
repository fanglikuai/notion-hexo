---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKYLH5U7%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T130103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIQDKtIE2nDcuxdOI6ogLCAOsBTndUR0dSXO5hWmQNAx8vAIgfqbXYK59RZKcY0UOS%2BnyE267XvgXs5TkQu9xWdOClgwqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW%2B3XwiZOsK70CKOSrcA8H8VD1VibYHSIh419U3IYpq27HfZSPuiqMTnpokWdPTB4yHlXCwsATR8LqvyBu7nk%2BKnFAksfHd3iTdxXO2Igv88zPF7kss97IL47MiJMpIcdwV4j1080DO4r%2BC5dCoy%2BLTlyviRgjJ5%2FolSatUq68tbGxzDixH3FyvDb7Kagvcgw28Unse%2B6aQ4rDZoT%2BxYFWjHcIEQyoAkXORq%2Be7xBk6lrbZMJa1p3seXuIRWBfOr%2BxDrXKxQimhlWG7lmsBH2OtsnK37192r2dNaQ%2Fzevvp9yH1CAfbotg6dt1yibcNXOBk9f9uhPjig9qgiDmfmMscIcRkRxiOWxXug6XBumm052F%2FzMQKZGYXVowJLpaKhj48PIbvNckganbY9y1NT4EBaXgEmxeGguZRL96gw%2BTnjL9s8v6HSJUbJssNzGjE%2FAyPmyuv4yN6MD6Qvgw0TEQ6SVgowC8eZ1VHZklMdVKwElcsnG%2F%2FhPz9JApI8rUbg644gGrgsJKwrTYw9pK0c%2Fjc0v%2Bxc3HXR9crSIgtBIy4MFe5X43zNT9UByyMHnqtbRVHqRmxFsBxUiVINV%2BFzVv2Zq%2FrkrgW8nKVlFpVFnPcyHficgDb4JxOdfPzWpH14pxVjCQIwO2Hur1UMJTVqsYGOqUB0KENV4u7fW3Bswh323xaHte4cTq%2FRJDz19TGHzLt3bN3JpjV2UN1uKYuPNfFUOm60vCq%2F%2Bw777MrUnK%2BqDYjP6v1Ti1Pi%2BU7pWa8P2s44QRRzzyIieeDYEKOVfb8RaWvEb6aQzTX0lTtIdL6THlAQPBCUPnSFTPmjBLWo3LrfpofzGaIFtqCN6p27O2yfgyTd%2BxP4s0J3ui%2BywdZGgqyXbSMG8xJ&X-Amz-Signature=72dd3806a03f87851ec31187d00a73bfb1528814331daed1891ce71c71403a1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
