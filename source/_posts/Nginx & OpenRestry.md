---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SJL4URA%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCmSwusZmUbuFTmMwF1qwchWCNTe5Wtg0ZSUH0pKNMuTAIgYRY7QJBL26G%2FGyfmR0Vhvx7DIS4tOQqEpVAZCwYx4XgqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOxBqZYd%2BzeeM2iJcircA51b0vYWWhaYkD7PoAP%2BaeRa2J2h4JeTLl%2BKhTHVvS2afXlLdJcTNi01sdYxvnGPfMR03kN9D24t4IIVv2Le%2F4zROpcmxDlvKhBhnuu3ze5hHseEZZFp2F8SnnjnMDdIcMZqyghSWuDY%2FAXJX11BZOQxwJ7Q1AS2nKdcs%2BTO%2F9VekSWefseftUWe7NCJ2JeT7HiUxx96qxVqux2UJQVt3fciKwkOEWaaFYhaO7gz6GhY8Jqq2dQAsd%2FYfgTJTin8DiZc%2BIMFEBJ40XfvJVIkC%2BHyV%2BVBNW1DkoPlFXfEMSeJFtaU4cShVcqMgwCMFxWrGbd5RYM7J1Tod5t5N0Eh36QXG5hweNJi2san2YA7MBC7%2Fs7E5rKVm39Cibly1ca3tsLZXLTH%2FNpeNK5UEqLT%2BkvuUed%2BPhEmTsNGr%2F1iJt2AQEtuHJmMAs5E0qHsLRmbJxnDQStTVJV2UQcVLxsrquDxh%2FAH8VtM1%2BBBGqs86hR76KncTohL7s3SwrEMs0CDkOKQl1%2BudEiYh%2FhwbhAYUgZ6eo1Q17nd3JJDClitoRwwerd7SK%2FSCIOSToPb2o2nbYf0fXhW9g7yF9%2BeAxudZ7GUnGZOLKELZnQ4LAiqLFBWnByBSO%2BRFfJLd1MNMNaThcgGOqUBQW%2BvqPtv6A93LFSKH5DS6m0SjBtRwx62iico02Qp4z9GugHu1%2BjPoU4vxJN02td0xx7FA6ba5njP4NEvdKmijbtG0MZI1s7aOZ30vZo0kzklNdMBW4ors4gnRLeUbfx0e%2BCQAybDWEJ0azeNadn%2BChgZDgq9gW8e0oxvYyd1xvLPMwaVAbuEuZyipV50OzOlsgFp8KuSQU74o96g5patjTccbb2c&X-Amz-Signature=ca1335b77a9a7531b8459f69e3b6ad94cc564ad6bd6ae1a589f63aebfe2bcccb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
