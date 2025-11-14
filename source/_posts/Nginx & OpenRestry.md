---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXE3ZVK5%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRbPWGI1RCOan0m37wrwDMNJPnICYx9BVMF2EBbtvSswIgfCEKb3sB9QOrtGk9ZU9UK2tliRu4IRpauszNtoGUYloq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDOb%2BjWVG4tY3u1BHdyrcAwLt93OWhDT2FFCYGW1pVDjBhCGH4ImkwUHWIRgzPUgKoWNpVfDgEoUpIHbwRc9tlr8pKs8KpCmFc0LYhA5%2Fqd4HIWk4RT2deuXKpfXpGqeQmDERisSLKxdUk%2FtYFBly4IFtqvscDRaoZ5U6AA5WHO5B8yQnhuBauTD19a%2Bq2n13YYmObldLETCbtmcapWBOPKsJjQY%2Fie0iR8%2BTWxPb54Hxv8XV4tZUmwgMBPFuyjaP3X56jip14mjAqc7QqWe6jYiERmIKUNKIy%2BD84MAjqQepbugURULJHpXpPGm8mEbZiwr1jbvRF7pP8LpJcOarSUofad%2FNc%2BwbGBH2KoYv%2FFPVyJoFFk3xqDUlhKPh5oYs47ZRaYUedBu682sqdVKkzWJil%2FtCZ9M8s6dp3zSyTWevB28cO07nj8z5%2BDmQzU55IXm%2FatGYRTcxGcWZODZWKObPVDNNglqPFey8V06Ai4jzkb2AmAroEexel57eZl7PC8%2Fy3MNZLs6XLEfzEovw8sP5CPcbQId5zuyj5B702ve9eCne60rb%2BNcPyMdt%2BCflvlCrHifYbiC9DAXKmrURijjW1ygPO0OsEN60s6mDqOt5QEcg2DujKEOhiARIehcQbnapaV%2FuxVzKMkPOMJKB3MgGOqUBT6UsnZVOf5wfVXru6ybhsws1CyaZMngloMv0LtssAKTdiXDWM1ffkMPimRd9HlaGbDX5CKC5GbGM%2BjJPUp%2Fk0AdFl%2Fx8cnqEvsRKQNicOZkj1PAjc6okPSnKfrZlJ%2BDq%2B5925R9A4andZFrWN9%2FtQIz0yTO8UXG721Nt90Gojv%2Ba%2FJwyU%2F4qcN7H8Yb5aPJxWt5CezSb7kZ3UjtE86Rr8kscX0LH&X-Amz-Signature=ecc4f03b03b7604d3f6446657ce4f2eeeb0f3d9c476115afb87240b100b104bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
