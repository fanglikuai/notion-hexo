---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH3FP6BP%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T220053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrAnINkiKuMGMbb8iaviM8oxxFsHJ3NJN62LKd%2BgGG8QIgEGpLfIKx%2FAYPadWCVUrADYzxou7HIM57HpKQHv5P1uYq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDKxpGmwSQXfKpUDk%2FCrcA8aKCF7idn1gn4FfnRoDZ4lzwKD8x91xCq43zJbO4n1lCaFnvNdr%2B1%2BqM%2B21NPCwZAV40iT1W0J0gw7QLGczbpvy09O1vDFGRMLNFAJKoC8wEOeMlG%2BPRLfxIf9kSBG%2BGpT%2BA86ACGd23zUw25F6j4OzGfl7XPN0FtQB6VWc5p5GzMRFdQaZ%2BeEqZIJIleLWN62wefR%2FP4B5%2F1qL6WlHc9%2B90nHkl6LxW2tLyiy%2FDpmsOIGO5I2XzEA08XOZSDkQErocQnSooAZa%2Bcz4Ay%2B%2Fn95UlxxO%2F00BrobEBDAOIpD%2FItWkNFc7S4X383gygIA7cljT3c6KsVqX7zp3%2FIp5l84VMb9EpeHJ9eZzW4xX4fjNMckN5iZH2onwkAZTv%2BIWGs3qJZv9RmzX0E4Gq4fQHNIt7DSL52ZzGwJrF3jZRe8zvCpKYGOlL78eCJ3R6lB0%2FcGBfZMs2x3lJcU2IVBqcnLkf0kX7relXPfC1aDFkjnH6cccyLObbg4aPcSfA3eousP%2B1Y2LLNG94ZgYYNkXgr%2BnCBZBeqeP26xqLBTM30ZF3fvPohWBf0rCrByZ33yAVRfk2ibwvRLiESVuFHx8BgCdQ5uP4TRywPwA9PUXOx4iR9S7IOIOKB%2BMgClzMMn788cGOqUBe41qSk12UgVw4ho3MBLwkVkYfULfaPwEaIX6inWGUNt8PhIENWUC%2FBiV3so7ADfFEdicRGUFR%2FaaEEIc2SduAYCPm0eqL0PMTqml7EVlXkvoZvUU9Qofi%2FbdHzrp1lndtWA0gG7tGps%2B5H%2FJk%2B%2BYdm%2BinxWrUOVzBfCiLu8%2Fw1px9zGk%2BGbHYIhX1HCcoDUVlKKo%2FSfQOXbPFhaoEbpVuIwyPn1H&X-Amz-Signature=21ec214b2379f5e4266c99c4a1264ee0505e6930545b88a5973501442a43e7b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
