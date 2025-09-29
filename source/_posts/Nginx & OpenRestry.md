---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZBOUAKS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQDLIjaspxtruHv%2FNkGASlFoWhtU5Lv0xK8LGQHlLBmHuwIhAI0gWTKowHF9gtNREmj4pDstR1ejzs4zPlffqvsZSe4rKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxTlcyhP9TzC0NgCkUq3ANeF1HC6pOfz%2Bs0ftTXz7sE%2B8Liyt%2Fu4BuH7qHBxzO9RpSkgu4rxDzhWGHfJZ14eWnPh5fXXnNT39f2%2FdVYzRpZVuWJSMeyW2EEg2PwtP6XPBxxH10xNA4ts%2BF%2F9KCMJmpq6ZEy2e6hn7XAIr5aDALObET6acKKdxhl4ujv6PqCTQvvrna3RpKZAuOvMh%2F%2FOUMJfbmhjhLiUbmo6iDptUx2F6AeH1uQ5eSc91sL2bL1ThunK2qk%2FoWPNWPjUq%2Bxk9%2B0%2BDPoAO45JR39DEOB0X%2BmX1v9IhcdLOGGX3v2%2FOF8K5JVCnr2BiBS00u3V4TfHWc%2BujdNM7bbYAzROzJzrgGHxE4sE2OULHDhP7lIJNRFbIrbvz5RKhMLpvZwdA%2Fwv8%2FvQO%2Bavl5s0B96no7TyPL9WTMvXok6zTvFeB1u8POmTElZm23X6tgemcwXswGQ5%2BeJrpMEmVj9qKubWsnqi28r3DYh7BkBHZHM%2F2rk7Hma8jTRaY%2Ftg8LeDbjZQMXDNoxduGw7QeDgm2LdbB9lVnLOa9n31xu1XvIzOlW6lToQDGjd0vpqv6Qp8CmhLGVxFJm7edxMl%2FZbUvpbtfa9hPeQw2mkcPEaYLRPxSCefhZo7QwYdBMcxRJB%2BRMHBDD26evGBjqkAVR1Ib3sFV1H4Mmgmh4hL046agO6gUMb2%2Frnuvxy%2FIsb%2FDQWqo9lky7JBpoKTkOB9ZogBQr%2FDdvWjLWA3J2wLjokV%2BGdiVU3%2FWx4wQPGIYTmWHJ0Vb0SrSKKM80WXCn7ELRhrwLgdW8%2Bxigj9Hk%2Bt2nyxKpB0BRe1bUvv5pX2fpz3ws0BA8nAewH8%2By3pB%2BPEdX0lc72vpqxJOmkcG2Vvxf%2BJP06&X-Amz-Signature=5cbd5e7d815cb89142dd7cfef3551292a389e29f3224eca42bacf7cbcb001b44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
