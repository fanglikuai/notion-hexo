---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC6JIBKL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T050107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZcgsKtX8bnSiS5ldOx3kuZo6BB8ycu2yGz35qAv3POgIhAKyNtWlhbjBy7J9feJoZDilFQ8Kbe0wAeHL%2FOYxJAiTXKv8DCGUQABoMNjM3NDIzMTgzODA1IgzBaEmSKKn367GzmOYq3AM2gloLkjn646PUild3OlDkSB2GTo2TyovmtKl2uxmQNCF%2FBXsFjme%2Bu3eFpR0OAhfWEfbqnjDsSVzynRc58slCLbhJxXgMxIA9ro0tepCd4HmQgep5m95SCP9xE7O%2BPKV4bA0d%2F7tjj6eptyMLpdO3YhO4HMM38%2FGmEflZn8kx%2F%2FhSS73wsvAB8lfF6ukC4eSiA2yhQqjA3KJaMehBD9wmW33nUt4la0%2BekU01hTrS3ttDahJYdS15K5fmMzLJ3jM0K8r4tfGVgnS9uDBeuXq5yijnvTrI1sA1StE%2BVshiJjP9TFnxTRWqvQAre7lLFQ3GJyMZf6Ro5rapn%2BfFvVZZchS%2B6fkP3sB2dYCY9jIJl9gZHhbrCNgXX4CdA%2F7nLljvcQcNVXtqenUPjqq3pU8LQN0eGmLeal9rkFlPsko8K4jy09t2aj%2Fw321tVUjajU993Las2C8KGBsfNSTijXFCCKM7qfWUoXpBxApBeAIwf5R4SnbCCvX%2FtPnqfeboBKPFZv8h%2Br8IqakE1byAkHmWdEP4Bs%2BtiiLFkpdUwO%2BU6F3oL4qyMFCDg3hh9dezfRr5CUtS6m02o5obIq6XyL%2FFNrrgYpxAM4cFC2Jk9CgCCgzeKKWgIl0wXAOwRDDi2pTJBjqkAYYWS%2FlXtvAMAowMRZEgp1FIhy1UTlFPHdT9euhoi72pJ%2BfjASB6YNlBehLjvEMJAKxPjLHQTWeU%2FfjCbP6ml7608LSOK9mhSJ4gQdaEkVGRHvUxuH98nxq55OA1zJik8TYpEFzEDNAkBnVjNLKFUoBavflO4wRtEZjM4RNYS37%2FqM6txGPrIj95Np91pVoMKf4eDbsS8ZcKWXck0PkbbqzpsFkE&X-Amz-Signature=d8de673865c7dee41d8f86a25538e7143837ca682d7345eee8afff55e7c7c5af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
