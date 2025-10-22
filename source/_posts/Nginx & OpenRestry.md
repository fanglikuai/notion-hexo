---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHB6W5ZW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIHLtbS0cVWiUzgUjXMvcVEGVOffvy%2FeUK%2BuuwiNbiT5zAiB4eR6P5UWtXbyKcbNfucGZBq%2FabeUm%2FH1%2BDReEQ2njZSr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIM1c8k27%2BCOShOITY1KtwDkf8U8XxVkiarqRWyyhdRsYF8OnXqFFQsZ0raKEmRZGI6z2b6bDVoqOf57ntT2nwHiqj72vq%2BxtQeu5rEJLOsQiTTMe%2Bh7zoZR2fNBYkc4ERijgsiBX4nXHny2cEtJFYlb8jRANYQq8nqvIEO6v8Is4fItbmxtmS51lHSic0yfvA1Rkw1pUo52A2AY0ZIwjOf%2F%2Fx9CANCWgeIIFVOgCsSFxVm1KvujHyKG3boacsrZOLAkjuY8g1SzZAqm0boCjO%2BtyLaTUWW7OkxYdHGUJshjnhqzt1Z0IcUgw8RNyGRG0kQpBRxF7PgEddzn5NWBXAiHS65CFJmpF7frELkYOtQ%2BrfKZiSm6GuAFcPsEe5liUp%2F4MZWGlvzzfaZQ%2FbIy88HP8L9TtLgUMkryHWqhY3HkuzU76SQY087qj4136y31dh7r4Xo6q2bS03YvDocC4neZGOqbUSxd8hkblRcpdYoxDDFX%2FC%2FfJdDJVIFsu5%2F%2Bhjye1yCgDajNHFefAhTSC4U6hmw4se%2F93ajhRaDqZHFo8rUTAR8sNj8ErwG9FbKAkSrZmttaK3AyXAsY7RAcHftD6BnIhrV%2BsELkCuZpSrzrXBEIDq5EtLuYbwukqggyvqVbgBd2nOQixKI23ww2engxwY6pgGzJCESSqXZxMnlzA7LLeJaGzouUPIwdNGBn24FBPgVHE5ylz0WlibmKoh%2BwoleGGuK%2FOkaeNeOqtbkNPNjFgmOyqwrYwBCJK%2BpPdoxf0u7KFptDBF90eQCCIgqbUTkDrGpfErvvI2m%2FPi6QUftcRZn7HafM%2FEAWqC1o%2BdiZVbQsSeHvMXln0QS3jGjZI5SrK0iUAYS%2BXc9Xbg%2BWdgEDcNTuudVUOBY&X-Amz-Signature=de5317aee8d8b23d3c24d9a32dd6e153b4d9d3d11eefe5a98e6d41ad946ae79e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
