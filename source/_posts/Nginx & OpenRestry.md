---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SITPB572%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T090056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDYYQ2r1Affc2N38Bm7xfXup2idm8HFjx6TNMi2SGhV5wIgWm7FtwupgUSs9Hb3Y%2BACzQrPAiUPyRDMFBBIsUsUd7sq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDG6sgehEWA%2FwE6bpICrcA9PK7%2BIfQ8wuB3RRMBss1kdFFkNc3pR18tzTwB12nf7nLvuDiLfgGgbeMB9ykF%2BwPMPC36hEnoAxiaMqKddHPZUy4l3JE8A57FvV9jdcEqYoLPE4AWXPSHR79088RqWRMUR3D5hvU27Eh%2FKOA5GXo1KDOs5iUTODjA%2B7CcuoxjwQMBx6lTWcXCW%2F0%2BrPoPvjq3lQRIXLeyQI%2BVq6SIijB7E3eSt5b66VCoRVyEqacqNrSdUIrXMBPtwZoykp9FNLrnpdShDh8DhIvOphXHpSg4eRW%2BOzeXWfhqWYu6QkummZnLvDprYhlHz%2B%2BRGu2hxNkg10J3q27gHTF%2F62b1ffMnS22HUwhR9wyno6s3otjaERlTggaudNmid6Qt7eL53DxCJEK4LjQc9NzSHy7T202h32I2pR6R6ZmJPLOMMSc%2Fsn5VfI%2BDyQTZoBGKgho9O3L9of9F4lcgGyQyzIsIrPnz428HbWrdVyKkU%2FHXkBnyGQiPW%2FPLIcfDmtZgfdP5c4QY05AxHBbPmOUGMOATt%2FKRFeejifiYilHFydOzpZ057TAzHDLQ1RDiMvm6gELLZOZK%2B2RhhQ3vlUE0WOO6zoOryLpkhod6tis3o5JNUj0NVyALHXOBZUmZ7%2B%2B2fmMNeXi8kGOqUBV6wJSTNu1jf9MgD%2Fu6zH5q8rFmSyt3Dnz%2Fp1h6XiyHgDrZFLQ4KvvjlqaWl1Uo0CO9i1k%2BojqpTbH0wChyssPxGqQ1JsMSbNF%2FDfSTQ6mu5JI2BYEccDagoH%2Fpkjx3fWmA69FaxnyHef%2B%2Bl4NF9VAYy6D4Rkhb9FQW4sXQd5l5MusrST%2FFv48puU%2FWMhJnOYAWT9cDbDxv5Gq9IhJlaE39ocgdNi&X-Amz-Signature=651bcd59bb6d5c5255a1120f2926c931d460a9bb4545d05f1e04c6d44756955b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
