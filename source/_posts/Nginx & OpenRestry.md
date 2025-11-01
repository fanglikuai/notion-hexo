---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MBH7YJJ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCICwHABGq3i6hmBqm2tICfZ5yR%2B8t0ksi3IbG3NTAsCPDAiEA%2FcUBQviMVyZMy2qbpolN1AQzLuhzg8y6brpVR%2FC%2FmB4q%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDNU08iELQviy%2FMOpJCrcA3MbdbKEaYrdYPeBcEffa5RwNgulC8FWsFEvDjuyKehopSEaOLrWPnZKzljlXwhLXAVUFIw4s63ZEugOt1F7zhd4yUCDG4b5uLqxvsIyxmtGjS2RsnFnjC%2B%2BcGNhz1Z93170SA4zRJmiQSi7xgGghoDfVuDftI%2FqwC1D8Lyxw9A4RqpifIxB7OLVbrh0oLtRAYC9Me0FAry1Mtb1QI3RghSlnD4x2LdcJzXvPX8aX4Ks7gkqeOxdNWGPOsfJbJWZ3LQh%2Bh7IH36yLsjYtAHdvcPVx0ZKTawO6N9cr9cRvNq1FqgVJ27heQI2MfIvsHynb4w%2BaYO3HK9108M5a9mPdQd59AqDA%2FRDLtnttF50ymV1%2Bzm498%2FlGAdu9pRS3L6lQu%2B7WfI3IqMU6bUK8zfO1SkDvoFjwGqXqPAb3ILj9nAU2xQEM90MsQASHcRC3NNnzDhabjdL9u58rHQEtRUAgYDYx2sqeSQ4WEnjXM1GYrevUIzm3wcTMbhUadDqvQIbfNBLhCKuucMI3etbWiSk9Quk7uwJDbI4h5gQGyirA3N21QQklZXZf2tjUDNCIbX3Fb%2Brk%2BFmoJsLA7F9ZMiug8dUNfEAfGGOBCj1s0rvk6XrpLiUeLyN8xi3F8NdMN7PlsgGOqUB8oSxnePqMhjXuntoZa0moa8vQiBR54sMzJOQ0qZpXiE05i3m%2BLvJKO54PFLMmKBvNXUnXh8x0vKJhUcGwcBcYateCN4yAxHdr0bHjRDgkGfBIyQEj%2BkQ6u%2F8fQcLwSdmgzD4XPEViEROh3RRRtIZwk7Ky%2B%2Fa2s2Pn1Labh56c6mfkXYqfXjZ40oy5icKSaPr3zFztYX5X81uAKVX5AdZXlzU5bsl&X-Amz-Signature=405f8e91b700ad839efbb494defcaa90ea3b9d813e5b2cd55a8eb62dd4ccd5f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
