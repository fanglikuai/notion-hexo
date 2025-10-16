---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRZJBB42%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCz0%2FDdC29sNZ2dp4zn%2Fz%2FUKtQU91xNvE1IAB32xwOqgQIgHs7XjnGqqzTvgurUd2K7RlWuVWBRPKhbKTGjpOitXskqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHZWCPQevMfmX8CEVyrcA2wSHbCVcy5BhK8QVY0Wj921W6zM3Bk041KRRkL4kU8aMtXLtXzpjRHu1VufLRRjT3nQzi2G1PuBQO3IPngOW9yRDpVLS1hvouBgiMNxRo9L43%2BeObnll8S%2F4DVxcYjB%2BXyMRNG0ey6u9WMe2oKAqrlBKmgIPyKuVBxfQLHu04JmaP9Hfr%2FEhs2QZnGRlsp3D9QDraGTQixMlYlK7nn1uqcR8N6uWHXGs2i4krvt1d3SWlQSIjWW1Z7Oixw6w%2BEaqsgglGyEXsE9zgj1PkzEchoSakTxRZn%2FUBqwCpqMeOUf%2F0QmPqQUn8YvdBIya%2Bb3K0vMGaB%2BQJ3jDRL5i%2BZJIJrcFUgCO7KKoCeCNIqvfpxv%2BtQ5SWJdOUAfoHO1Bz%2BJC8AuXR40fh1xbOQ49yXzMV9%2BeB19toiELF7DmF8NVvl7lBJDyYGF8BOdpEjnXMKSWAve68B4Vani9eAG9QAA9JU%2Bmfs73WNPbqPReME74PH1WS%2F8D5yOWAdPj2x29wOO4yOzv5wr42mk0IP1gKR0amCCHB5OLP4lct2bIoI57urLqT%2B%2F9t6IMO6fSeGuKtLQX2EnWdLdriS4dCC99ckZVBgIxl71POt26fv%2BQof5ULkjMApYmFcWQBZ0rSW3MILYwMcGOqUBN3fmJYqgUDFUljrGs3LbQokZOaFEM3EwHxLKvCTrrDKGt7ClwWZyasgHPyFubwUaLZ%2FZjtdrnfZ8cVgHWpk7cFZUD8Ipq%2FJ%2BRNKzHYyrrLH8EeTM02zUfET%2B%2BXYrixbUKNXLObCqWLFR0SRvgysvFfu3Kxkf0TlGTp4F%2Bq4p4nO4afckCVMTIsksQuijEunRp8g1MBApVGoA0ArEBfwnNj%2FYzxbc&X-Amz-Signature=52a15bc0cda24dfae602572223c91524a134fed74c236806ff31e8b94cbccc90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
