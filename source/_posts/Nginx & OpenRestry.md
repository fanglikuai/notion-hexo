---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665D6B224M%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T020101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDy9m7wN2p9SQF%2FfB6rXOFkPgr4YeCqs8IYIm93MoYA0AiBgkGecCMFDJnCM7WP8RmVpV451PzmYbdeLDdeyouvFtCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMauzdm9tMM0Nr1pxVKtwD1QPzpt8MBQb9jTFQb9cPTosWNjUbN3KWNdP9mBKYuLRRvpkj3ksy6gXMvFrz1dKe9QE6wfiTIZRx2AVAhnqIekNqO42%2BegClQqh2l0fozkquJob0VXtj9gLfUypUeBI0UFSNdpPZwmEDSW9Yn6qaU0vBuhAGKxfMGSgWTHanxP0s1gSULTZ%2BK3RzjljfDoLqIFPNUpvLjPgr5l04wxngEMeQwPX47bjZXXc5m0jM4LaVvUw1PCIuwpeVLy%2B5WV1em%2Fji1tLttRzSyJukpDAG8JSTWOsJ07IzifjscUCCKCELVRrY5dFStWjywzmn6ZNGHU%2FojHctdrlFO%2FO5tBgmsBg1skN12msG%2BcMBUmyfKq%2BrQNc03%2Fa2XyBgqs41VuX9imRkadVNDhofKZy8uIaIotpa%2Bzb5qSyB0u1nJeZs%2F8f%2FmYTkHuzG6%2Fi%2BHgKYvTOXuqm2UrxA0QdKodDb94pU%2BEGcyyRCC6S5wvGlhRy3FM4%2BD7aLgHEY1OwwsdykyD6IQumWJxS0XJVE1gi2rH8Bnibw0I2%2B55bN4%2BR2pU4k%2B9YmUt855ljddbkDbpphbMa6D71hlTDEPlS1Ixn9nsyjZYo%2BYxCf9FvO3EeFKBsgIZOrC0fgFqSeE%2F3gajAwzJSZyQY6pgHKDLDEpRuM%2FyyxDUmpmewgfAy7nnzhQpAPIdVCCofVvBSpNDAt0%2BaZSia6MDNlg7o62ibF8IxRwI37wwcEXeRgyc%2FQNWv0XLWPMUcLTGg79YLN5wEOpg8iQZaDHnIsLhpZpawwMvdeChObKMpNMOvfCeyV1TrARNwa4uweWfWXYZM1OyZDhCa%2BeudZOoBw1eH7o%2B8L5FImvBIjcQ5X2EROjHJGdDvn&X-Amz-Signature=a55233bd20173b0a266eb03793155b3419609875e99a5a8f1f6704f0b131e77c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
