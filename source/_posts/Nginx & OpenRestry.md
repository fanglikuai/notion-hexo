---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZXKS2KYI%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBetCnxvmxzMm36C69fjX9557MBuSUCBcZZUCLrb4OiuAiAkbioLa%2BqoiX%2F0dKHeLTzscJ35fvGZmUzQsDRDVtjlPSr%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMl7JBZsmVfBfrne8vKtwDNz3ls9Ouqr60%2B%2BMc8scTHPgLlaYqMDG0d%2B3AJkL9ZUOi0blUsumgNjL%2B%2B4mtuF1FEFuGilA03zThiyoufvjUiOJQvZSFhvdk%2FteQo6x6ip01pMEGMR%2BhMv9vsAWxYh5a8c1%2FkwliPnIDEHMuujRvG2QPFAXTNe69NtjwLsDGLf9AUmUVRqed2W4migWfxsEMwFVKLgnE8l7cXCG3sJV1QYssmw%2FEFuwY0EAsbzBpDU6pkwModyR9tf9dLlcCdQj%2FN30ur%2BIGbGXCIEwZsCjc4w24St1YA960HFJOz2zMW9dIxNf%2BR7g5S8NCzudSTopDKrnqKXWj98XMw%2BgKYeCaEKmsAj90oz%2FlpRcTlVaDNDKMXtMBfaLEq8eagvd%2BsaS%2FQx1a1iAfP24lCRnk9w%2F51aE3a9NiZzzWznUdA6IVXaN3ztEUEpddcmDnWrwltKRfqJ%2Bt1JdlIWsfOQfbn9UN6TOaaCpqhPd94UY6yErhEQPlWnMSWFzTn69KZU4C8%2FPz4Aj6GO2zaVMXNzE7JWTiw0whSVmlOFR4NBfyl4qGSGoyRbXhN%2Bb8smmNAnZPb9SISz0JHfKrPjiTTEZODJo9dgMQ48UPy1scPtG1TrsLv7i%2F8%2Fu8I3EuOQv9qfcw772eyAY6pgHj%2BN3h2Xd1pYL2AkSV0mY58MIy8zWMF0M0F1jp3jpFSYq9mbfO9ycAXlc6H%2FWE1QRHK4NpRb%2BE%2FjGz1bihc%2FvRCZ8OifFaxaV61FV2IVvBd%2FXfY%2FYBSUelnARcDEVPalQnl3Um1UoZuz0rPaD6we7i7kWKCg9zH7GQG9B6LrGlFU56y7Rt52GX%2BxZ8SK0UMC8%2B0bY3DsySNwZE0eQTLAcsHQsnyQQf&X-Amz-Signature=c87ec5a6d52a97f354a8cef58d8cabadb9cbf8b5954bf7c00f478356415371f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
