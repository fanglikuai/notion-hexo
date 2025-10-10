---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DII3ICX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCICCgKQEs27Yap3f5vzAMA7qQtpfWusgbsojNxd2n02f%2FAiAaiOAGvEJPqB9AkcP3vTaAi2JpshaR4gJW%2BJ7qhqOyWyqIBAjl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLHO6o50r7rPjj%2F7xKtwD0czDOsMwe%2B%2BPS20jaBhiOpRt1BcfkfRuL8OJpLTW%2FVmnKUJg7OxMJDbsEJWW10TQPFXn%2BxLo07HnDT12gOk2uEdwgfxbe8GZ%2F%2Bj%2F56olwrcJaEKG8SF7fgeatrW%2B5HWFr0bCYwH81qGspb4P6eIRoAQcKtVGkqQKcLbIbJetV4b1b22s3v1eg9e1mq2pFauk5NUOp7DJd8DYr%2BR3bLBuiG2vJCyUfzT7gQmEtkgxxf7N09G9vUEU57VlWr%2BT6pP%2BiQWYtn3k%2BDvr6Qz8LcxyEoWHF%2FsJcSFfFGipLKqS6tKzDHtKPsSUsLRxtDbMROuyFWppi2MRG61DjBopXCv6d7igwNJGnTGirgQBz8tWhR0UjrcDVZ4pMeGTT8fYGJQNjTunY%2FWIWsErG6Rp5ZEF6wuC7nfebY4n%2FssjLp5PtP3SU2KKUMYorXFW7ojqQZV3MwXY%2F3vLtx7gv2GrnP2l2uReDDKe3P%2BjTzfPyI7NBZoXWUr1SIgbHmNa7bETtPA8CtoUKhtSGTiLZjXhymwUQ3bDCYK0clV6zBO37%2FVjmWjzYkHFImn4EyJgZChDunVqpLeIN8F%2FRl%2B6j0FT%2Fq0Z7SGVPve%2Beyf2WeJZsBplZsm5MxooaCXGQ3bnnkEwrPyhxwY6pgHdpu%2B6MpAbt1jkjDfOUPhvOPKKobUbuldXy2fPysc%2B0%2BFJK%2F2vaGPEmT61wBnSP%2BRFEQ0EZkqHAmve4Nf8QlT7DSffTYoohJ1DC9dg24MhrxhogIOgTuKsgglZ20JWMwpDugCxJlq0vnPsJJU1Ojb79dKfESk8%2Bi%2Bnis9HU52N%2FEHUlzpyNAtCiwA0qH6yLmj1JmZZT2BmYZmLILqGAHef9%2FikoEGb&X-Amz-Signature=8dd23579b3091c0a4e2f5f9d4c8a301c356c95ac1f657c8b9a331ee9f4483c54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
