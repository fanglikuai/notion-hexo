---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEIOB7CH%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJIMEYCIQCzWDEY3EyqTZlbL2c1jlfPApLweTsnkYJKy95LdDpAagIhAKyLt%2BqSaGM5CrdwIUQ33C4BiBaFHAb%2BCqMIR0zVVzX%2BKv8DCB8QABoMNjM3NDIzMTgzODA1Igyd5hhzLA4y89Igeysq3AM%2FybRCH4CEIshhNSJFcQnt17dMDAeEgxlwBUvnTG%2Fp4Fi7kIGDFuvdRkPRUA98d0SufOboHosHez7kqlxyB6eki97UrOm5Prc1P%2FlsAuoRc5385zDzsuBmW2W9lHqxYrHqxk7S6bKMWPG7X02Ln%2BW1A%2Flf5cNl4ksoYAGYdfg8vKI1mTZHJ0epRgErWJ%2F1tmGJ6lRNGdPOa0g%2BB%2BGmxjSyWp75JNBEenUlmD2azF2WTFVuWToy96r1FKi%2FWvxJ73CHr4vht2CC9F17Qwg%2BIE05LgLZEuVUppwU%2BX4YeLuP5%2FkQd%2BQp3amY7M94FjDfdpYJMra%2Bntx2RANOrt%2BzY95u0%2BMA4knXbEHGzQkiVEWncDJDRAyx%2FmzRnDxX6XdL713xM8S9qiSzrdUyeefbuLDuhSqlCWWtk%2BH9siwNxV1D91G5GopW2BoBaDDv4onNOQ776KOJu9O8coSN7mQb4Rx6thJZxMoiw4YxtHtoU6r0APYxnycJkRIN4ri2hwtSQ%2FSovXhrXIinQt9kDx0qq4wx%2FRWD3BeA%2FbNaKNeOtexsn5mTtDc4xucwrb1T69nLi9kRLsVxFFq1izE5h%2BMpoL8cFMY4Q6d%2Fy2Pgvmo%2Bbh1flV7qu%2Bx3UbyfF147jTDfpqvHBjqkAZrB7Nzt0WUjkamo5xjy1qY4d18v815yV%2BiYUZGjL6vGD1qvETmt0uCgV91ZbpTBQRV3w1al%2Fz20FHhUxpYBP9rqRCFjQrKTjUD7Hr1MkNOmky7WX%2BkFOD9Xff0DFOB7GPAHsOwSULgkc01YHaf216cR5arnROToV2joNXYGJLPo9UMym50rE4ea5PBVn92ZS%2BrvyyuT8vxeDZSWLpol64zji3N5&X-Amz-Signature=dbf8b1f29c40b02b4f25b65f1549b5683ddeb62d39540440f12b49acdd632de4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
