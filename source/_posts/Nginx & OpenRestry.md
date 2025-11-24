---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SGZRUSCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T220113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICcyaYecxAEBNnyBy738c0Je4e8rQ%2BrbOe2IXEkvmexvAiEAv3GJecZH4C%2Bbz8zP42aaIhkUFPJCMjewi60Ua8G5Obwq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDDCsqYNVvafZxLkRDSrcA2vBaxZFaoHa4bWAUOFAfe%2BE94CjJy8oy6UATHxD15TFiCs1hG%2FkDyhFMGxQOEk1KBSlTMdiyQyiZ6A0wMClRSW8zjVCK1GygXjLrciOe68LO9L2Fx09vmmWenmjf1B2HqBaUziPJsSkZiSFHHCRyjMuSsRRPl2MCRu1Yqw4W1LfNScn5RBdk91sjoLzSpIoR%2FWIwmkjleqYXLNSRvEqbasp9KsqIc6fZ0acBgBLkNIH7MJbppy9SkUFRvQIFZ2B2qhCHDYy4hYJNVMqGzgMI1LYk11MiNXGwvJyJ9pGyE5RGH3rlcxzMKjX1uq7fMDQFHA88OWT5cIWrkQ%2BpReLXNTyX%2BtlaeElG9neo%2BRh3cq0G0C4%2BBlNFWTgTk9CtroNW2DjUW%2Br7BxkZ5fysoCEzJG6R%2FLUy20u50R8PgLjLpiTEmnKURMpi%2BtrGSwnPQIjy4r3T7LruvPZAHD9qGOdlzSiyiGG39Y1SMSGD6B%2FdlkgqzD0PnR1YR3br8IHX1xdUnnBIGx9NY%2FQUA3jft%2Fd2riHMHCrL7kufOjd7pa6uXS%2BPdYoEbkucxfMvjW7RXhaZLElKS18Y4kTLWql715Wt1iuTNeyTqBF1fVDkXamIt%2FMIHPU%2Bi47bIzOxoO%2BMLuUk8kGOqUB9fNc8ZXZjQMFEdXb2hYcWxCpQVNtH7pAVYxBdwi77kyRp0oDS0%2Fj6CtgLepCPnKOZMyPf5XHS8ZIt7AJxJfPODRfITNXfS4jPsxK97HwB9dCqmlfymusThjGzzyAYU0C41mNfIOZZ6EijY%2FdzdIJ0CbzfBABQAZAJJk8ppjcqsRWqpXxg6UbnxEvbqvd8y5CrqEFs5GFQp02eokrXIuW5j1FKA17&X-Amz-Signature=9625df1e3df1069cf803f1f1fdb6ac6ba2790bb62327a9ec9be953cde4fe019f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
