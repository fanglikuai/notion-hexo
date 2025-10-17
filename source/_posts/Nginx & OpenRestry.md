---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666STS3PTY%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCICoSp7U9vOS4zu%2FK46mrF2gERzZuEIhzAIbe4F%2FlINMKAiB4a8JKpes7AyENF3AYmC1td5aJjZiA4BdDBBbrXzLsOiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMzDV%2FruiSf1sq1CNTKtwDyXoMrT4xz0yNthHsZ1wZDfUPiPCmDAgCKGYKBmi1HcB1TtHBaAOTnCmW%2FF9zlFAneMVFz2C7TKz0%2BAH9wp%2F8QPGtXyNtNgYG41q3KyWTBB3d68ulGS43LRE3Iqp9Ks02YRTNzIfvNXMSkG8zGq8%2FDjxOXg3yMdrvdB89XD5hWRt4isws9Fv47BxjzXXGsAclANcjZcMWf7fpVlIGsKGG5PLJRcU1Jyda%2FMu3HJInz8TTiLRuQQ%2BxGy%2FixtDb%2FmBK6wXWbTarw4WDRzSxas7SE8v1DUGvB5hB7dU7cqRoP%2B0H0XnW0%2BddaQLtu3nB4ORTQR7wQRgJcW4OuVg5ce9evOWI3FknwfYf1%2Bt3hEGHEypt2XUBLRW%2BYH9ChCJstj6%2Fc2JusBVtVapI7BQjPtIkRzCLeFWFNbgw5iR%2FXcaHlz3FLj9nd9Ny2YJPhpG8hhokuasMLMJ4vDExoUF%2FcolE%2BdkcN1pA4BmTBrAFlpi611hWOW8tkz9mFj9SqdNpnwRAfPkbGHicK38mdNk7b5fCjM0Ro55DWtYNnOokzmoDINYkKny7%2FicrYarM8kLLPN%2FQMDcfhr2TG4K5sg6nD2TMl1te%2Bfw52%2BG2HtaVDfHYTlrqrQl%2FaXm%2Bl%2F%2B%2Fg5Ewuf3JxwY6pgGOfkz4MD6BNl5%2FBuedgOdDf0D9YmwDB3T7wNM6%2BDFMnIcFWK1i7M1FkLHFkol%2B1j4T54f9SU8Yr7gvbVT%2BgNgY243HaP7WkrjezjiyQrh2VSQmsBoOfeIdnMzmQYGyy4xMmvfdhTJ2hOwtzrVqnL7RS2yft%2BK5tjUcOs5KbGrwUUEyAWSu40op9JLfY31fdjhkGrhvsk8xVbQollLEe2HlbMldb8uE&X-Amz-Signature=ac34b018fa254b5e0e03ee2f808831bdad4b847f7f8e76f1320620545214d539&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
