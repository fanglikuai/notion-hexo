---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZZI2FBJ%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQDdN%2BWGpRFAx2i8Ct%2BOC8W33ScvfM3pjzxAEKzWU6kZpwIgMuOpQdVJP8Aizci6h%2BbhLU65sGomv2YjUGoHds%2F7Q04q%2FwMIARAAGgw2Mzc0MjMxODM4MDUiDG3C5R52zRvyzOMQsyrcA%2FF2Q3AMsE87%2FW2o4j3UxGJyFLaXKlm%2FIw5%2BvaqCRgshXjMX1lA37jr3ptfMsIOa0tjX%2FSSyR%2FJ0hKsLZfbI6lpHHvfuezgU1dZm0EpibnK%2FLbwPG6hwwEq9%2BLWvQ%2B8p3oPbmN0AXpsXvW9dSL1KPEwR5N5ag7yNHxIP2fQ26ghbRco9sy67TBpcpLngFDyYsIFnJjtzxj5g3VApTvboRV3W7ogoB367KAR%2B%2B0Y%2BKEZ2P1ksQaaJmj3HMFA%2FTAv8xO%2BK3E%2BtRSsH6o3LQP2BbfmyvnIvksIXJ0TFydJgDWIMT9nznyWRXMOyWHC1U9SIt8DNAl5Bsd83LWgZrETijn85pmFP5bI57Wu4DO8agrh6xou1hJ%2FLlzlfCQlKo8sQvxBJO0tM5qCW2IL6dWz4fcw%2BAZ8vQlFcjeepE%2BHYvyNw2eo1TDNd0e3%2BnoHXumTGbxmaSwMbkB6xGBtV2fQFM8uRJY2G9mQLo1FlSeHvE%2BkcK2nFCyechIN1J3zeBEUa4C%2Beni2Iu0i%2FHlDpSzN2eTykLF%2BshcCDvM916a5Pi0qeZoJEq1CvqgHcQoZQuMVCDUbRc4biZr6xKR8ikQt8p5g%2BM0U%2BjLO%2B3UwMkhksKP2KprljJNDlada7LX%2BsMJy3xsgGOqUBcgx1WAS9YtA8TTj6Q%2FOpz64cUJoorU3FbnRPWz6r0mUkHXO42OELrjjEq0924%2BGEbS%2B7rUPmyAfiDUxyQTAuEwE294APvMQi018gB1VOZUUqNkHYIAm19mDbqxOxxpjzmo70K47S8vVoPayruayFkNLLEotWGmqhhrxecINYPz80wXb7wU5apz40ICBT2jNiH0BTCUk4FlJKbHAYV1f12e0mRNWu&X-Amz-Signature=601f5a0ef196d18c85389fd4d0baeaa590c716c599475d2d9cd17d3ef3dee89d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
